# Get chemical SMILES strings based on CAS numbers or names

Here we have a few sets of codes that you can use to find chemical SMILES strings (structures) if you already know the CAS numbers or the names of your chemicals. Although there are many sources available for us to get SMILES strings, mostly they will not cover all your chemicals if you have a lot. In such cases, checking multiple sources/databases becomes necessary. 

In this repo, we present four ways to obtain SMILES strings, the first one being using an available Python library called `cirpy`, while the second to the fourth ones being three very popular websites that are built on very large databases, including http://cactus.nci.nih.gov/, https://echa.europa.eu/, and http://www.ambinter.com/.


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
The website `http://cactus.nci.nih.gov/` is another powerful and easy-to-use source for looking for SMILES strings if CAS numbers are given. To use it, simply add the CAS number in the url as shown below. Then open the url with `urllib.request.urlopen`, and the decoded content would then be the SMILES string.

```
>>> from urllib.request import urlopen
>>> cas = '108-95-2'
>>> url = 'http://cactus.nci.nih.gov/chemical/structure/' + cas + '/smiles'
>>> smiles = urlopen(url).read().decode('utf8')
>>> smiles
'Oc1ccccc1'
```
