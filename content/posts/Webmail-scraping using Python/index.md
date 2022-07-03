---
title: "Webmail-scraping using Python"
date: 2021-09-23T07:59:25+06:00
description: Webmail-scraping using Python
menu:
  sidebar:
    name: Webmail-scraping using Python
    identifier: Webmail-scraping using Python
    weight: 10
tags: ["Basic", "Multi-lingual"]
categories: ["Basic"]
---

Python as we all know is simply "Awesome". It can make seemingly complicated stuff, glaringly simple. So, I'll start this piece with a little background on how I came up with this idea of scraping?

I recently came to know that my college's email id will soon be closed as I have completed my coursework. I was certainly horrified because I literally had 10k mails in my inbox with some of them being really important to me and I couldn't afford to loose them. Hence I decided to save each of those mails for future reference. I accomplished this with a simple python script using scraping

After reading this article you will find yourself comfortable in scraping web pages using Beautiful Soup, selenium, and pyautogui. Moreover, I have used other standard python packages also.

This article scrapes the old webmail of the Indian Institute of Technology, Kanpur, but these codes can be easily extended for other web servers. Although IITK, students will find it easy to replicate.

Basics of Web-scraping

Web scraping is simply the art of data extraction from web pages or websites.

The general process before scraping a webpage involves:

Inspect the webpage and find out different dividers, classes, tags (check out the different types of tags and their attributes), etc. used by the developer.
Start using selenium, Beautiful Soup, etc. to get the output in the desired format, that can be processed easily.

Now, we are ready to save our mails, before the email-id shuts.
Imports
Start over by importing these python libraries. If you face any challenges with selenium, try to have look at this.

	import pandas as pd
	import numpy as np
	import requests
	from bs4 import BeautifulSoup
	from selenium import webdriver
	from selenium.webdriver import ActionChains
	from selenium.webdriver.common.keys import Keys
	import pyautogui

Complete Framework

Let's first start by logging in to the email. This piece of code uses your "Username" and "Password", to get into your email id using selenium and pyautogui.

	driver = webdriver.Chrome()
	driver.implicitly_wait(5)
	driver.maximize_window()
	URL = 'https://nwm.iitk.ac.in/'    ### use your webmail's URL here
	driver.get(URL)
	username = driver.find_element_by_id("rcmloginuser")
	username.clear()
	username.send_keys(Username)
	password = driver.find_element_by_id("rcmloginpwd")
	password.clear()
	password.send_keys(Password)
	l = driver.find_element_by_id("rcmloginsubmit").click()

To open a specific email from the complete array, we locate each mail and click on it.

	save_path = Your Location
	messages = driver.find_element_by_xpath("//a[contains(@href,'_uid={0}')]".format(res)).click()
	time.sleep(10)
	pyautogui.click(800,200)   ### this coordinates might vary
	name = driver.find_element_by_xpath("//a[contains(@href,'_uid={0}')]".format(res)).get_attribute("innerHTML")
	name = name.split('<span>')[1]
	name = name.split('</span>')[0]
	After opening the mail, let's go ahead and save it.
	filename = os.path.join(save_path,name)
	pyautogui.hotkey('ctrl', 's')
	time.sleep(1)
	pyautogui.click(200,200)   ### this coordinates might vary
	pyautogui.hotkey('delete')
	pyautogui.write(filename)
	pyautogui.hotkey('enter')

This complete piece of code will help you save one email at a time, you can run a loop to save all your emails.

Conclusion

The objective of this article is just to get you started with scraping and this approach will surely not work for all the webmails. Just make sure to inspect the web pages you are planning to scrap and the above methods can help you achieve your goal.
P.S. Here I have used pyautogui library from python, it allows you to perform all the keyboard and mouse functions via code. It was new to me and felt it will be helpful to my readers.
_________________________________________________________________
Do share your feedback with me and if you liked this blog post, do give it a clap. Follow me on social media (Instagram, Facebook, and LinkedIn)

