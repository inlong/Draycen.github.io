---
layout:     post
title:      "症状推疑似疾病"
subtitle:   " \"python\""
date:       2018-07-20 20:00:00
author:     "Draycen"
header-img: "img/post-bg-alitrip.jpg"
catalog: true
tags:
    - python
    - 自动化测试
---

> “python ”

* python 获取excel里症状数据。
* python 获取excel里症状对应的疾病描述数据。
* python 获取excel里症状对应分值数据。
* 输入 分值 >=50，的症状，点击疾病描述，看能否推出疾病。

```python

# -*- coding: utf-8 -*-

from selenium import webdriver
import unittest
import time
import os
import HTMLTestRunner
import jb_list


class Ctsy():
    global base_url
    global driver
    
    driver = webdriver.Chrome()
    time.sleep(2)
    def setUp(self):        
        base_url = 'http://.........'
        driver.get(base_url)
        time.sleep(3)    
    
    '''快速问医'''    
    def kswy(self,begin):
        
        zzlist = [u'发热']   ''' 疾病列表 '''
        jblist = [ u'皮肤隆起性紫癜、结节性坏死性皮疹']  ''' 疾病描述列表 '''
        fzlist = [100.0]  ''' 疾病分值列表 '''          
        try:
            
            for i in range(begin,len(zzlist)):
                if fzlist[i] >= 50.0:
                    
                     
                    time.sleep(2)
                    driver.find_element_by_xpath('//div[@class="scroll"]/div[1]/div[1]/span').click()
                    time.sleep(2)
                    driver.find_element_by_xpath('//*[@id="foot_wrap"]/div/div[1]/i').click()
                    time.sleep(2)
                    driver.find_element_by_xpath('//*[@id="foot_wrap"]/div/input').clear()
                    time.sleep(2)
                    driver.find_element_by_xpath('//*[@id="foot_wrap"]/div/input').send_keys(zzlist[i])
                    time.sleep(2)
                    driver.find_element_by_xpath('//*[@id="foot_wrap"]/div/span').click()
                    time.sleep(3)
                        #driver.find_element_by_link_text(u'查寻疑似疾病').click()
                    a = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[3]/div/div[3]/div/div[3]/ol/li[1]/label/span').text
                    if a == u'查寻疑似疾病':
                        driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[3]/div/div[3]/div/div[3]/ol/li[1]/label').click()
                    else:
                        driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[3]/div/div[3]/div/div[3]/ol/li[2]/label').click()   
                    time.sleep(2)
                    driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[5]/div/div[3]/div/div[2]/label').click()
                    time.sleep(2)
                    t = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[6]/div/div[3]/div/div[2]/div').get_attribute('textContent')
                    time.sleep(2)
                    t1 = t.replace(' ', '').replace("\n", "").replace("\t", "").replace("  ", "").replace("1.", "").replace("2.", "").replace("3.", "").replace("4.", "").replace("5.", "").replace("6.", "").replace("7.", "").split()
                    if jblist[i] in t1:
                        if len(t1) == 1:                    
                            zb = t1.index(jblist[i])
                            #zb = zb+1
                            driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[6]/div/div[3]/div/div[2]/div/label').click()
                            time.sleep(2)
                            try:
                                f = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[7]/div[1]/div[3]/div/ol').get_attribute('textContent')
                                time.sleep(1)
                                print zzlist[i]+':'+f.replace(' ', '').replace("\n", "").replace("\t", "").replace("  ", "")
                                time.sleep(2)
                                driver.find_element_by_xpath('//div[@class="buttons buttons-left"]/span/div/button').click()
                            except:
                                f = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[8]/div[1]/div[3]/div/ol').get_attribute('textContent')
                                time.sleep(1)
                                print zzlist[i]+':'+f.replace(' ', '').replace("\n", "").replace("\t", "").replace("  ", "")
                                time.sleep(2)
                                driver.find_element_by_xpath('//div[@class="buttons buttons-left"]/span/div/button').click()
                        else:
                            zb = t1.index(jblist[i])
                            zb = zb+1
                            driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[6]/div/div[3]/div/div[2]/div/label'+'['+str(zb)+']').click()
                            driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[6]/div/div[3]/div/div[3]/label').click()
                            time.sleep(2)
                            try:
                                f = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[7]/div[1]/div[3]/div/ol').get_attribute('textContent')
                                time.sleep(1)
                                print zzlist[i]+':'+f.replace(' ', '').replace("\n", "").replace("\t", "").replace("  ", "")
                                time.sleep(2)
                                driver.find_element_by_xpath('//div[@class="buttons buttons-left"]/span/div/button').click()
                            except:
                                f = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[8]/div[1]/div[3]/div/ol').get_attribute('textContent')
                                time.sleep(1)
                                print zzlist[i]+':'+f.replace(' ', '').replace("\n", "").replace("\t", "").replace("  ", "")
                                time.sleep(2)
                                driver.find_element_by_xpath('//div[@class="buttons buttons-left"]/span/div/button').click()
                                    
                                    
                    else:
                        ysdbs = t1.index(u'以上都不是')
                        ysdbs = ysdbs+1
                        driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[6]/div/div[3]/div/div[2]/div/label'+'['+str(ysdbs)+']').click()
                        time.sleep(2)
                        t = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[7]/div/div[3]/div/div[2]/div').get_attribute('textContent')
                        time.sleep(2)
                        t2 = t.replace(' ', '').replace("\n", "").replace("\t", "").replace("  ", "").replace("1.", "").replace("2.", "").replace("3.", "").replace("4.", "").replace("5.", "").replace("6.", "").replace("7.", "").split()
                        if jblist[i] in t2:
                            if len(t2) == 1:                        
                                zb2 = t2.index(jblist[i])
                                zb2 = zb2+1
                                driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[7]/div/div[3]/div/div[2]/div/label'+'['+str(zb2)+']').click()
                                time.sleep(2)
                                try:
                                    f = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[8]/div[1]/div[3]/div/ol').get_attribute('textContent')
                                    time.sleep(1)
                                    print zzlist[i]+':'+f.replace(' ', '').replace("\n", "").replace("\t", "").replace("  ", "")
                                    time.sleep(2)
                                    driver.find_element_by_xpath('//div[@class="buttons buttons-left"]/span/div/button').click()
                                except:
                                    f = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[9]/div[1]/div[3]/div/ol').get_attribute('textContent')
                                    time.sleep(1)
                                    print zzlist[i]+':'+f.replace(' ', '').replace("\n", "").replace("\t", "").replace("  ", "")
                                    time.sleep(2)
                                    driver.find_element_by_xpath('//div[@class="buttons buttons-left"]/span/div/button').click()
                                    
                            else:
                                zb2 = t2.index(jblist[i])
                                zb2 = zb2+1
                                driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[7]/div/div[3]/div/div[2]/div/label'+'['+str(zb2)+']').click()
                                driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[7]/div/div[3]/div/div[3]/label').click()
                                time.sleep(2)
                                try:
                                    f = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[8]/div[1]/div[3]/div/ol').get_attribute('textContent')
                                    time.sleep(1)
                                    print zzlist[i]+':'+f.replace(' ', '').replace("\n", "").replace("\t", "").replace("  ", "")
                                    time.sleep(2)
                                    driver.find_element_by_xpath('//div[@class="buttons buttons-left"]/span/div/button').click()
                                except:
                                    f = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[9]/div[1]/div[3]/div/ol').get_attribute('textContent')
                                    time.sleep(1)
                                    print zzlist[i]+':'+f.replace(' ', '').replace("\n", "").replace("\t", "").replace("  ", "")
                                    time.sleep(2)
                                    driver.find_element_by_xpath('//div[@class="buttons buttons-left"]/span/div/button').click()
                                    
                else:
                    print zzlist[i]+'   '+u'分值＜50，略过！'    
        
        except:
            try:
                ttt = driver.find_element_by_xpath('//div[@class="scroll"]/message-dialog[3]/div/div[3]/div/div/p').get_attribute('textContent')
                if ttt == u'没有找到您想了解的信息。':                   
                    print zzlist[i]+'   '+u'执行失败！--------'+ttt
                    self.setUp()
                    self.kswy(i+1)
                else:
                    print zzlist[i]
                    print jblist[i]
                    print t1
                    self.setUp()
                    self.kswy(i+1)                        
            except:
                self.setUp()
                self.kswy(i+1)

             
         
    def tearDown(self):
        pass
 
if __name__ == "__main__":
    Ctsy = Ctsy()
    
    Ctsy.setUp()
    Ctsy.kswy(0)
    
    
```









