```bash
!pip install selenium pandas tqdm
!apt-get update
!apt-get install -y chromium-chromedriver
!pip install selenium --upgrade
```
___
```bash
!apt-get update
!apt-get install -y wget unzip
!wget -q -O google-chrome-stable_current_amd64.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
!dpkg -i google-chrome-stable_current_amd64.deb || apt-get -fy install
!wget -q -O chromedriver.zip https://storage.googleapis.com/chrome-for-testing-public/$(google-chrome --version | awk '{print $3}')/linux64/chromedriver-linux64.zip
!unzip -o chromedriver.zip
!mv chromedriver-linux64/chromedriver /usr/bin/chromedriver
!chmod +x /usr/bin/chromedriver
```
```bash

```

```bash

```

```bash

```

```bash

```
