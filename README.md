# Get chemical SMILES strings based on CAS numbers or names

Here we have a few sets of codes that you can use to find chemical SMILES strings (structures) if you already know the CAS numbers or the names of your chemicals. Although there are many sources available for us to get SMILES strings, mostly they will not cover all your chemicals if you have a lot. In such cases, checking multiple sources/databases becomes necessary. 

In this repo, we present four ways to obtain SMILES strings, the first one being using an available Python library called `CIRpy`, while the second to the fourth ones being three very popular websites that are built on very large databases, including http://cactus.nci.nih.gov/, https://echa.europa.eu/, and http://www.ambinter.com/.

Please find all the codes in one single JupyterNotebook called **`get-chemical-smiles-by-cas-or-name.ipynb`** in the `code` folder for details.


## Python library `CIRpy`
`CIRpy` is a Python interface for the Chemical Identifier Resolver (CIR) by the CADD Group at the NCI/NIH. It is a very powerful tool that can resolve any chemical identifier to another chemical representation. For example, from CAS number to name, SMILES, or from name to SMILES:

```
>>> import cirpy
>>> cirpy.resolve('Aspirin', 'smiles')
'C1=CC=CC(=C1C(O)=O)OC(C)=O'
>>> cirpy.resolve('108-95-2', 'smiles')
'Oc1ccccc1'
>>> cirpy.resolve('Oc1ccccc1', 'cas')
['1336-35-2',
 '108-95-2',
 '63496-48-0',
 '73607-76-8',
 '61788-41-8',
 '14534-23-7',
 '50356-25-7',
 '8002-07-1',
 '27073-41-2']
```

The installation is simple:
```
pip install cirpy
```

For more info about CIRpy, please refer to https://github.com/mcs07/CIRpy.

## Website http://cactus.nci.nih.gov/
The website http://cactus.nci.nih.gov/ is another powerful and easy-to-use source for looking for SMILES strings if CAS numbers are given. To use it, simply add the CAS number in the url as shown below. Then open the url with `urllib.request.urlopen`, and the decoded content would then be the SMILES string.

```
>>> from urllib.request import urlopen
>>> cas = '108-95-2'
>>> url = 'http://cactus.nci.nih.gov/chemical/structure/' + cas + '/smiles'
>>> smiles = urlopen(url).read().decode('utf8')
>>> smiles
'Oc1ccccc1'
```

## Website https://echa.europa.eu/
The website `https://echa.europa.eu/` is another useful source we have been using. To use it, we first go to the address https://echa.europa.eu/advanced-search-for-chemicals?p_p_id=dissadvancedsearch_WAR_disssearchportlet&p_p_lifecycle=0&p_p_col_id=column-1&p_p_col_count=1 using the library `selenium`. Then use the `find_element_by_xpath` command to find the search box, and input the CAS number into the search box. Upon submit, we follow a few more steps to extract the SMILES string (if available) mainly using `selenium` and `re` (regular expression) to identify the target content. 

These steps control the automatic opening and closing of the web brower, which usually takes some time to get the job done. For more information, please refer to the JupyterNotebook **`get-chemical-smiles-by-cas-or-name.ipynb`** in the `code` folder.

## Website http://www.ambinter.com/
Similarly, we can also scrape the website http://www.ambinter.com/search/ following the same steps to get the SIMLES strings based on CAS numbers. Details can be found in the JupyterNotebook **`get-chemical-smiles-by-cas-or-name.ipynb`** in the `code` folder. For example:

```
from urllib.request import urlopen
from selenium import webdriver


url = "http://www.ambinter.com/search/"
CAS = '108-95-2'
driver = webdriver.Chrome(ChromeDriverManager().install())
driver.get(url)
search_box = driver.find_element_by_id("quick_search_input")
search_box.send_keys(CAS)
search_box.submit()
try:
    links = driver.find_elements_by_xpath('//*[@id="resultArea"]/div[2]/div/table/tbody/tr/td[1]/a')
    href = links[0].get_attribute("href")
    website = urlopen(href).read().decode('utf8')
    details = re.search(r'<th>Smiles</th><td>.*', website)
    details = details.group(0)
    smiles = details[details.find('</th><td>') + 9: -5]
    print(smiles)
    driver.close()
    
except:
    driver.close()
    print('No results found')
```
The output would then be

![image](https://user-images.githubusercontent.com/70991409/138579421-dba691df-a638-4d58-86d2-16770c7bb9e6.png)

