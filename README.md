# undetected_chromedriver

https://github.com/ultrafunkamsterdam/undetected-chromedriver

Optimized Selenium Chromedriver patch which does not trigger anti-bot services like Distill Network.
Automatically downloads the driver binary and patches it.

* **Tested on version 75,76,77,78,79,80,81,83,84**

* **patching also works on MS Edge (chromium-based) webdriver binary**


## Installation ##
```
pip install  git+https://github.com/ultrafunkamsterdam/undetected-chromedriver.git
```

## Usage ##

To prevent unnecessary hair-pulling and issue-rasing, please mind the **[important note at the end of this document](#important-note) .**

<br>

#### the easy way (recommended) ####
```python
from undetected_chromedriver import Chrome, ChromeOptions
driver = Chrome()
driver.get('https://distilnetworks.com')
```


#### patches selenium module  ####
Needs to be done before importing from selenium package

```python
import undetected_chromedriver as uc
uc.install()
from selenium.webdriver import Chrome
driver = Chrome()
driver.get('https://distilnetworks.com')
```` 


#### the customized way ####
```python
import undetected_chromedriver as uc

#specify chromedriver version to download and patch
#this did not work correctly until 1.2.1
uc.TARGET_VERSION = 78    

# or specify your own chromedriver binary to patch
undetected_chromedriver.install(
    executable_path='c:/users/user1/chromedriver.exe',
)
from selenium.webdriver import Chrome, ChromeOptions
opts = ChromeOptions()
opts.add_argument(f'--proxy-server=socks5://127.0.0.1:9050')
driver = Chrome(options=opts)
driver.get('https://distilnetworks.com')
```


### important note ###

the default blank page on start plays a BIG role in the anti-detection workings of the module. You will only become undetectable from the moment you use driver.get(url) to navigate to some url (and next and next and next). This automatically means that if you enter a url in the browser screen by hand right after launch, you are NOT protected! New Tabs: same story. If you really need multi-tabs, then open the tab with the blank page (hint: url is  `data:,`  including comma, and yes, driver accepts it) and do your thing as usual. If you follow these "rules" (actually its default behaviour), then you will have a great time for now. 

TL;DR and for the visual-minded:

```python
In [1]: import undetected_chromedriver as uc
In [2]: driver = uc.Chrome()
In [3]: driver.execute_script('return navigator.webdriver')
Out[3]: True  # Detectable
In [4]: driver.get('https://distilnetworks.com') # starts magic
In [4]: driver.execute_script('return navigator.webdriver')
In [5]: None  # Undetectable!
```
### end important note ###

