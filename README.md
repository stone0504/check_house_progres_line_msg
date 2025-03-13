
# Webpage Change Notifier

This Python script monitors a specific webpage for updates. If a change is detected, it sends a notification via LINE API and captures a screenshot of the updated page. The screenshot is then uploaded to Imgur and sent as an image message.

## Features

-   Periodically checks a target webpage for updates.
    
-   Sends a LINE notification when changes are detected.
    
-   Captures and uploads a screenshot of the updated webpage.
    
-   Supports configuration via a `config.json` file.
    

## Requirements

-   Python 3.x
    
-   Required libraries:
    
    -   `requests`
        
    -   `beautifulsoup4`
        
    -   `hashlib`
        
    -   `datetime`
        
    -   `screenshot` (custom module or implementation required)
        
    -   `zoneinfo` (or `backports.zoneinfo` for older Python versions)
        
    -   `json`
        
    -   `os`
        
    -   `pathlib`
        

## Installation

1.  Clone the repository:
    
    ```
    git clone https://github.com/your-repo/webpage-change-notifier.git
    cd webpage-change-notifier
    ```
    
2.  Install dependencies:
    
    ```
    pip install -r requirements.txt
    ```
    
3.  Create a `config.json` file in the project directory with the following structure:
    
    ```
    {
        "LINE_ACCESS_TOKEN": "your-line-access-token",
        "USER_ID": "your-line-user-id",
        "LINE_API_URL": "https://api.line.me/v2/bot/message/push",
        "homepage_url": "https://example.com",
        "target_url": "https://example.com/page",
        "IMGUR_CLIENT_ID": "your-imgur-client-id"
    }
    ```
    

## Usage

Run the script using:

```
python monitor.py
```

The script will check for updates every 7200 seconds (2 hours). If an update is detected, it will:

1.  Take a screenshot of the page.
    
2.  Upload the screenshot to Imgur.
    
3.  Send a LINE notification with the update message.
    
4.  Attach the screenshot in the LINE message.
    

## Configuration

You can modify `config.json` to change:

-   The webpage being monitored (`homepage_url`, `target_url`).
    
-   The frequency of checks (`check_interval`).
    
-   The API keys for LINE and Imgur.
    

## License

This project is licensed under the MIT License.

## Disclaimer

Use this script responsibly and ensure compliance with the target website's terms of service.