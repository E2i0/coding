# coding
void
from my_spider import url_downloader
from my_spider import url_manager
from my_spider import url_outputer
from my_spider import url_parser


class Spider(object):
    def __init__(self):
        self.url_manager = url_manager.Url_manager()
        self.url_downloader = url_downloader.Url_downloader()
        self.url_parser = url_parser.Url_parser()
        self.url_outputer = url_outputer.Url_outputer()

    def crawl(self, entrance):
        self.url_manager.add_new_url(entrance)
        count = 1
        while self.url_manager.has_new_url():
            try:
                new_url = self.url_manager.get_new_url()
                content = self.url_downloader.download(new_url)
                new_urls, data = self.url_parser.parse(new_url, content)
                self.url_manager.add_new_urls(new_urls)
                self.url_outputer.collect(data)
                print 'crawl %d:%s' % (count, new_url)
                if count == 200:
                    break
                count += 1
            except:
                print 'crawl failed'
        self.url_outputer.output()

entrance = r'http://www.nh87.cn/caimeixunguo/2016/ABP-490.html'
spider = Spider()
spider.crawl(entrance)
print 'crawl finished'

