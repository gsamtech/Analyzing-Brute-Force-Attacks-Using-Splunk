# Brute Force Detection Project

## Overview
This project aims to detect brute-force login attempts by analyzing login activity and identifying patterns indicative of unauthorized access attempts.

## Features
- Detects multiple failed login attempts followed by a successful login.
- Provides statistics on login attempts.
- Easy integration with Splunk or other monitoring tools.

## Query
The following Splunk query detects potential brute-force login attempts:

```bash
source="brute-force-sample.csv" host="kali" index="bruteforce" sourcetype="csv"
| stats count by _time, user, action, signature_id, dest
| streamstats global=true count current=false by user, action
| streamstats global=true current=false last(count) as last_count last(action) as last_action by user
| search last_action="failure" AND action="success" AND last_count>=4 AND count=0
| eval failure_count=last_count+1
| fields - last_action, count, last_count
| convert ctime(_time) timeformat="%d/%m/%Y %T"

Installation
Clone the repository:
bash
Copy code
git clone https://github.com/username/brute-force-detection.git
Navigate to the project directory:
bash
Copy code
cd brute-force-detection
Usage
Replace the sample CSV with your own login data to analyze.
Adjust the query as needed to fit your data structure.
Contributing
Contributions are welcome! Please fork the repository and submit a pull request.

License
This project is licensed under the MIT License.

vbnet
Copy code

