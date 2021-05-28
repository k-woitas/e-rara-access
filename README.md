# e-rara-access

This little repo offers helping hands to **collect metadata and fulltexts of [e-rara](https://www.e-rara.ch/)**, the platform for digitized rare books from Swiss institutions. 

The Jupyter Notebook [e_rara_metadata_fulltext_access.ipynb](https://github.com/k-woitas/e-rara-access/blob/main/code/e_rara_metadata_fulltext_access.ipynb)
retrieves metadata in various metadata formats from the [OAI-PMH](https://www.openarchives.org/pmh/) interface of e-rara, and fulltexts from the e-rara website.

A static version of the Notebook is available with [Jupyter nbviewer](https://nbviewer.jupyter.org/github/k-woitas/e-rara-access/blob/main/code/e_rara_metadata_fulltext_access.ipynb).

The Juypter Notebook has three parts:
1. Metadata access via OAI-PMH interface with Polymatheia
2. Direct metadata access via OAI-PMH interface
3. Download fulltext files from e-rara website.

## 1. Metadata access via OAI-PMH interface with the Polymatheia library

*Polymatheia* is a Python library to support working with digital library/archive metadata. It supports accessing metadata of different formats from OAI-PMH and also offers methods to handle the retrieved data. The metadata will be turned into a Python-style 'navigable dictionary', which allows convenient access to certain metadata fields. See the [documentation](https://polymatheia.readthedocs.io/en/latest/) of the Polymatheia library.

The chapter makes use of the following OAI verbs:
- oai: 'ListSets'
- oai: 'ListMetadataFormats'
- oai: 'ListRecords'.

Then, it shows **how to access certain metadata elements** and how to **save and recover the retrieved data**.

## 2. Direct metadata access via OAI-PMH

Unfortunately, the *Polymatheia* library doesn't offer methods for *all* OAI verbs. For instance, there is no `ListIdentifiers` method (which delivers only the identifiers of a given set) and no `GetRecord` for retrieving the metadata of a certain item using its e-rara ID. That's where the common libraries *requests* and *BeautifulSoup* come into play, and more manual coding is needed.

The chapter makes use of the remaining three OAI verbs:
- oai: 'Identify'
- oai: 'GetRecord'
- oai: 'ListIdentifiers'.

It defines the following functions:
- **load_xml(params)**:

    Accesses the OAI interface according to given parameters and scrapes its HTML/XML content.
    
    Parameters:
    * All available native OAI verbs and parameter/value pairs.
    
- **download_record(ID, metadataPrefix = 'mods')**:
- 
    Downloads a certain metadata record from OAI to a single XML file.
    Throws a notice if metadata file already exists and leaves the existing one.
    
    Parameters:
    ID = E-rara ID of the wanted record.
    metadataPrefix = Metadata format to be delivered. Default value is MODS.
    
- **set_size(Set)**:
    Accesses the OAI interface and retrieves the size of a given OAI set.
    
    Parameters:
    * Set: The 'setSpec' short cut of the desired OAI set.
    
- **retrieve_set_metadata(Set, foldername, metadataPrefix = 'mods')**:
   
    Downloads metadata records of a given set and in a given format from OAI to XML files
    in a designated folder.
    Therefore it
    * requests e-rara OAI-PMH interface according to a set 
    * creates a folder for the records according to parameter foldername
    * retrieves the set's e-rara IDs
    * retrieves metadata according to IDs and given metadata format (default: MODS)
    * saves metadata to single <e-rara ID>.xml files in the folder.
    
    Parameters:
    Set = The 'setSpec' short cut of the desired OAI set.
    foldername = The name of the folder which will be created to hold the metadata files.
    metadataPrefix = Metadata format to be delivered. Default value is MODS.
 

## 3. Download fulltext files from e-rara website

Downloading e-rara fulltetxts can easily be done from the e-rara website combining the fulltext base URL `https://www.e-rara.ch/download/fulltext/plain/` and the e-rara ID. Note, that also fulltext paths to empty fulltext files exist and can result in an empty file download. Therefore, while retrieving the fulltexts of a whole collection/set, deleting of all empty files is already included.

The chapter defines the following functions:

- **download_fulltext(ID)**:
    
    Downloads a certain fulltext file of TXT format.
    Builds with e-rara ID the fulltext URL, reads the TXT and saves it to <e-rara ID>.txt file on local disk.
    
    Parameter:
    * ID = E-rara ID of the desired record.

- **retrieve_set_fulltexts(Set, foldername)**:
    
    Downloads fulltext files of TXT format of a given OAI set to a certain folder.
    Builds with e-rara IDs the fulltext URLs, reads the TXT and saves them to <e-rara ID>.txt files on local disk.
    Therefore it
    * requests e-rara OAI-PMH interface according to a set 
    * creates a folder according to parameter foldername
    * retrieves the set's e-rara IDs from OAI interface
    * retrieves fulltexts according to the IDs from e-rara website
    * writes fulltexts into single <e_rara_id>.txt files in the folder
    * finally checks all fulltext files in the folder if they are empty, and deletes those empty files.
    
    Parameters:
    Set = The 'setSpec' short cut of the desired set.
    foldername = The name of the folder which will be created to hold the fulltext files.
    
    
