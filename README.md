# crawler
import re
from urllib import request
import threading
import queue
import xlrd
global tanks
global lists
lists = []


class thread1(threading.Thread):
    def lists_output(self):
       i,k=0,0
       xls = xlrd.open_workbook(r'C:\Users\JuHao Wang\Desktop\htmls.xls')
       table = xls.sheet_by_index(0)
       while (table.cell_value(i,k))!=None:
        list={'page':(table.cell_value(i,k))}
        lists.append(list)
        i+=1
        if (table.cell_value(i,k))==None:
         break





page = thread1
print(lists)
page.lists_output(0)





class Spider_guns(threading.Thread):
    def __init__(self,url):
        self.url = url
        print(self.url)
    # crawler pattern list
    nation_pattern = '<small>([\s\S]*?)</small>'
    factory_pattern = '<li><span>研发厂商：</span>([\s\S]*?)</li>'
    name_pattern = '<li><span>名称：</span>([\s\S]*?)</li>'
    born_pattern = '<li><span>诞生时间：</span>([\s\S]*?)</li>'
    chassis_pattern = '<li><span>底盘类型：</span>([\s\S]*?)</li>'
    bearCap_pattern = '<li><span>轮胎负重轮数量：</span>([\s\S]*?)</li>'
    loadCap_pattern = '<li><span>乘员与载员：</span><b>([\s\S]*?)</b></li>'
    length_pattern = '<li><span>车长：</span><b>([\s\S]*?)</b></li>'
    width_pattern = '<li><span>宽度：</span><b>([\s\S]*?)</b></li>'
    height_pattern = '<li><span>高度：</span><b>([\s\S]*?)</b></li>'
    totalW_pattern = '<li><span>战斗全重：</span><b>([\s\S]*?)</b></li>'
    maxV_pattern = '<li><span>最大速度：</span><b>([\s\S]*?)</b></li>'
    maxVov = '<li><span>最大行程：</span><b>([\s\S]*?)</b></li>'
    condition_pattern = '<h4>结构特点</h4>\r\n<hr>\r\n<p>\n\t([\s\S]*?)\n</p>'

    def __fetch_content(self):
        # this is a HTTP request
        r = request.urlopen(Spider_guns.url)

        # this is htmls reading
        htmls = r.read()
        htmls = str(htmls, encoding='utf-8')
        return htmls

    def __analysis(self, htmls):
        tanks = []
        name = re.findall(Spider_guns.name_pattern, htmls)
        nation = re.findall(Spider_guns.nation_pattern, htmls)
        born = re.findall(Spider_guns.born_pattern, htmls)
        chassis = re.findall(Spider_guns.chassis_pattern, htmls)
        bearCap = re.findall(Spider_guns.bearCap_pattern, htmls)
        loadCap = re.findall(Spider_guns.loadCap_pattern, htmls)
        totalW = re.findall(Spider_guns.totalW_pattern, htmls)
        maxVelocity = re.findall(Spider_guns.maxV_pattern, htmls)
        maxVoyage = re.findall(Spider_guns.maxVov, htmls)
        condition = re.findall(Spider_guns.condition_pattern, htmls)
        developer = re.findall(Spider_guns.factory_pattern, htmls)
        length = re.findall(Spider_guns.length_pattern, htmls)
        width = re.findall(Spider_guns.width_pattern, htmls)
        height = re.findall(Spider_guns.height_pattern, htmls)

        tank = {'名称': name, '国家': nation, '研发厂商': developer, '诞生时间': born, '底盘类型': chassis,
                '轮胎负重数量': bearCap, '成员与载员': loadCap, '车长': length, '车宽': width, '高度': height, '战斗全重': totalW,
                '最大速度': maxVelocity, '最大行程': maxVoyage, '使用状况': condition}
        tanks.append(tank)

        return tanks

    def __show(self, tanks):
        for rank in range(0, len(tanks)):
            print(tanks)

    def go(self):
        htmls = self.__fetch_content()
        tanks = self.__analysis(htmls)
        self.__show(tanks)
    def __init__(self,lists):
        url =  lists


spider = Spider_guns(lists)
spider.go()
threads=[]
threadID = 1
for k in range(0,len(lists)):
    thread = Spider_guns(threading.Thread,lists[k])
    print(lists[k])
    thread.start()
    threads.append(thread)
    threadID += 1
for t in threads:
    t.join()
print ("退出主线程")
