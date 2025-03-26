from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.action_chains import ActionChains

from bs4 import BeautifulSoup
import time


start_time = time.time()
chrome_options = Options()
# chrome_options.add_argument("--headless")
chrome_options.add_argument("--disable-gpu")
chrome_options.add_argument("--no-sandbox")
chrome_options.add_argument("--disable-dev-shm-usage")

service = Service(ChromeDriverManager().install())
driver = webdriver.Chrome(service=service, options=chrome_options)

url = "https://vkvideo.ru/video-167225575_456240275"
driver.get(url)
def solve_captcha():
    try:
        wait = WebDriverWait(driver, 1)
        bot_button = wait.until(EC.element_to_be_clickable((By.XPATH, "/html/body/div/button[1]")))
        bot_button.click()
        print("✅ captcha done")
        time.sleep(3)
        skip_ad()
    except:
        print("⚠️ captcha topilmadi")
        skip_ad()

def skip_ad():
    try:
        skip_ad_button = driver.find_element(By.XPATH, "//*[@id='video_player']/div/div[2]/div[3]/div")
        time.sleep(10)
        skip_ad_button.click()
        print("✅ Reklama o‘tkazildi!")
    except:
        print("⚠️ Reklama topilmadi")
        pass


def scrape_page():
    solve_captcha()
    def extract_attr(selector, attr):
        html = driver.page_source
        soup = BeautifulSoup(html, "lxml")
        element = soup.select_one(selector)
        return element[attr] if element and element.has_attr(attr) else "N/A"

    time.sleep(0.6)
    try:
        title_element = driver.find_element(By.XPATH, "//*[@id='react_rootVideo_page']/div/div/div/div[1]/section/div[2]/div[1]/div[1]/div/div/div/div[1]/div/div/span/div/div/div")
        title = title_element.text.strip()
    except:
        title = "N/A"

    try:
        description_element = driver.find_element(By.XPATH, "//*[@id='react_rootVideo_page']/div/div/div/div[1]/section/div[2]/div[2]/li/div/div/div")
        description = description_element.text.strip()
    except:
        description = "N/A"

    try:
        video_player = driver.find_element(By.XPATH, "//*[@id='video_player']/div/div[1]/video")
        ActionChains(driver).move_to_element(video_player).perform()

        duration_element = driver.find_element(By.XPATH, "//*[@id='video_player']/div/div[3]/div[9]/div[11]")
        duration = duration_element.text.strip()
    except:
        duration = "N/A"

    thumbnail = extract_attr("meta[property='og:image']", "content")
    driver.quit()

    print("Title:", title)
    print("Description:", description)
    print("Duration:", duration, type(duration), list(duration))
    print("Thumbnail URL:", thumbnail)
    end_time = time.time()
    print(f"Execution time: {round(end_time - start_time, 2)} seconds")

scrape_page()
