# BORIS (University of Berne Institutional Repository) Dashboard

![UNIBE Home](https://img.shields.io/badge/Application%20Type-Streamlit-informational?style=flat&logo=streamlit&logoColor=white&color=red)

The "BORIS" dashboard is an integral part of the UNIBE Project Dashboard, designed to facilitate data retrieval from BORIS, the institutional repository of the University of Berne. This powerful function employs various tools and technologies to provide an interactive and informative experience for users exploring the repository's publication records.

## Key Features

- **Interactive Streamlit Application:** The "boris_app.py" implemented using Streamlit, a Python library renowned for creating data-driven web applications. The intuitive user interface allows seamless navigation and exploration of publication records.

- **Data Visualization with Plotly:** The publication records from both public and private institutes are displayed on an interactive scatter graph powered by Plotly. This enables users to gain valuable insights and patterns from the data.

- **Filtering Capabilities:** Users can easily filter data based on publication privacy status, allowing them to focus on either public or private records as per their requirements.

- **Institute Data Collection:** A dedicated data collection module "collect_institutes.py" scrapes all institute names and their respective codes from the official University of Berne website. This information is stored in "li_tags_data.csv" and is utilized to link the institute names to their codes for further requests.

## Files

- `boris_app.py`: The main Streamlit application file that serves as the BORIS dashboard.
- `boris_data.csv`: A temporary file generated by `boris_app.py` to store fetched data.
- `collect_institutes.py`: A webscraper that extracts institute names and their codes from the University of Berne's website and saves them in `li_tags_data.csv`.
- `fetch_api_data.py`: An extension module for `boris_app.py`, responsible for making fetch requests from the BORIS Advance Search API.
- `li_tags_data.csv`: A CSV containing two columns: "Institute" and "URL." This data links the institute names to their respective codes and is utilized by the main BORIS application.
- `requirements.txt`: A file listing all the required dependencies to run the BORIS Dashboard.

## Usage

### Live URL

You can experience the BORIS Dashboard live at the following URL:
[https://axfc4u8kxqgwuy82c9ifc8.streamlit.app/](https://axfc4u8kxqgwuy82c9ifc8.streamlit.app/)

### Running Locally

If you prefer to run the BORIS Dashboard locally, follow these steps:

1. Clone the repository to your local machine.
```bash
git clone https://github.com/gautam132002/boris.git
```

2. Install the required dependencies by running the following command in your terminal (inside the cloned directory):
```bash
pip install -r requirements.txt
```

3. Execute the Streamlit application:
```bash
streamlit run boris_app.py
```

## Data Retrieval Challenges and Solutions

### **Try 1: Using Scraped Data from OAI-PMH Endpoint**

Initially, I attempted to retrieve data from the OAI-PMH endpoint [https://boris.unibe.ch/cgi/oai2?verb=ListRecords&metadataPrefix=oai_dc](https://boris.unibe.ch/cgi/oai2?verb=ListRecords&metadataPrefix=oai_dc) for data scraping. However, I encountered a significant challenge as this schema did not contain the institute names, making it impossible to filter data based on institute name.

#### Approach:

To address this issue, I explored the possibility of matching names from the scraped data of the University of Berne website [https://www.unibe.ch/fakultaeteninstitute/index_ger.html](https://www.unibe.ch/fakultaeteninstitute/index_ger.html) with the names in the data retrieved from the OAI-PMH endpoint. I attempted to use two operators, `==` and `in`, to compare names.

##### Python Code Example:

```python
# Example name comparison in Python
name_1 = "Gautam Negi"
name_2 = "Gautam Neg"
name_3 = "Gautam"

# Using '==' operator for exact comparison
if name_1 == name_2:
    print("Output: True")
else:
    print("Output: False")

if name_3 == name_1:
    print("Output: True")
else:
    print("Output: False")



# Using 'in' operator for partial comparison
if name_1 in name_2:
    print("Output: True")
else:
    print("Output: False")

if name_3 in name_1:
    print("Output: True")
else:
    print("Output: False")
```

##### Output:

```
Output: True   # With '==' operator
Output: False  # With '==' operator 
Output: True   # With 'in' operator
Output: True   # With 'in' operator
```

#### Results:

This attempt failed due to two main reasons:

1. **Limited Intersection:** The intersection between the scraped data and the OAI-PMH data was minimal, resulting in a very small number (only 200-300) of matching entries out of approximately 140,000 records. This rendered the results inconclusive and unsuitable for the BORIS Dashboard.

2. **Inconsistent Data for Same Individuals:** Another challenge arose when the same individual appeared in multiple institutes in the CSV data, leading to inconsistent data representation. For example, individuals like **"Chiara Valli"** were affiliated with both the **"Institute of Communication and Media Studies (icmb)" and the "Department of Social Sciences,"** while **"Dr. Dominique B. Hess"** was associated with both the **"Institute of Psychology" and the "Center for the Study of Language and Society (CSLS)" and "Walter Benjamin Kolleg.**"

### **Try 2: Collecting Data from BORIS Advance Search API**

To overcome the limitations of the first attempt, I switched to collecting data from the BORIS Advance Search API [https://boris.unibe.ch/cgi/search/advanced](https://boris.unibe.ch/cgi/search/advanced).

#### Approach:

The BORIS Advance Search API allowed us to target specific parameters, such as the institute (input text), publication's privacy status (public input checkbox, private input checkbox), and more.

To conduct institute-based searches, I needed to pass the institute codes instead of institute names. To address this requirement, the `li_tags_data.csv` file was introduced to obtain the necessary institute codes. These codes were subsequently used to make API calls to fetch relevant publication records.

##### Bash Command Example:

```bash
# API call to fetch publication records based on institute and privacy status
curl -X GET "https://boris.unibe.ch/cgi/search/advanced/export_BORIS_JSON.js?screen=Search&_action_export=1&output=JSON&exp=0%7C1%7C-date%2Fcreators_name%2Ftitle%7Carchive%7C-%7Cdivisions%3Adivisions%3AANY%3AEQ%3A<YOUR INSTITUTE CODE HERE>%7Cfull_text_status%3Afull_text_status%3AANY%3AEQ%3A<SECURITY TYPE HERE>%7C-%7Ceprint_status%3Aeprint_status%3AANY%3AEQ%3Aarchive%7Cmetadata_visibility%3Ametadata_visibility%3AANY%3AEQ%3Ashow&n="
```

#### Result:

With the BORIS Advance Search API approach, I successfully gathered comprehensive publication records based on the specified parameters. This method provided accurate and consistent data for the BORIS Dashboard, enabling users to explore publication records based on institutes and publication privacy status effectively.

The second attempt proved to be a viable solution, overcoming the challenges faced in the initial approach and ensuring a seamless experience for users navigating the BORIS Dashboard.

## Contribution

We encourage contributions to enhance the BORIS Dashboard. If you wish to participate, kindly follow the guidelines specified in the contributing file. Your contributions are valuable to the continued improvement of this project.
## Contribution Guidelines

- **Reporting Issues and Bugs:** Report bugs and issues through the GitHub issue tracker.
- **Proposing Enhancements and New Features:** Propose enhancements and new features via GitHub issues.
- **Code Contribution:** Fork the repository and make contributions through pull requests.
- **Code Style and Guidelines:** Follow the project's coding style and guidelines.
- **Respectful and Inclusive Environment:** Maintain a respectful and inclusive environment for all contributors and users.


## Contact

For any inquiries or feedback related to the BORIS Dashboard, please feel free to contact me at [gautamnegi0202@protonmail.com](mailto:gautamnegi0202@protonmail.com).