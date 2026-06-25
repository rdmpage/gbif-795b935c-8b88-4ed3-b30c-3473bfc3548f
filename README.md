# A DNA barcode library for mangrove gastropods and crabs of Hong Kong and the Greater Bay Area reveals an unexpected faunal diversity associated with the intertidal forests of Southern China

## Step 1 Create dataset on GBIF

Create a dataset on GBIF using registry API. The **owningOrganizationKey** is the publisher UUID that you see in the link to the publisher page: https://www.gbif.org/publisher/92f51af1-e917-49bc-a8ed-014ed3a77bec. You also need a secret **installationKey** provided by GBIF, and you also need to authenticate the call using your GBIF portal username and password.

https://api.gbif.org/v1/dataset

POST with `Content-Type: application/json`

```javascript
{
	"owningOrganizationKey":"92f51af1-e917-49bc-a8ed-014ed3a77bec",
	"installationKey":"**<your key here>**",
	"title":"**<title of dataset>**",
	"type":"OCCURRENCE" 
}
```
RESPONSE

```javascript
"**<UUID of dataset>**"
```

We now have a UUID for the dataset, which lives here: https://www.gbif.org/dataset/<UUID>

## Step 2 Create and validate Darwin Core archive

Now we need to create the Darwin Core archive, which is simply a zip file):

```
zip dwca.zip eml.xml meta.xml occurrence.tsv
```

Next we need to check that the DwC-A file is valid using the [GBIF Data Validator](https://tools.gbif.org/tools/data-validator).

## Step 3 Create endpoint

Now we need to tell GBIF where to get the data. In this example, the Darwin Core Archive file is hosted by Github as a release. 
We can tell GitHub to ignore extraneous files by adding them to `.gitattributes`, but the files created by GitHub are not the ones we want. Instead, take the validated DWCA zip file created in step 2 and add that as a binary file to the release. The direct URL to the latest release will be `https://github.com/rdmpage/<repository>/releases/latest/download/dwca.zip`, e.g. https://github.com/rdmpage/gbif-795b935c-8b88-4ed3-b30c-3473bfc3548f/releases/latest/download/dwca.zip. We tell GBIF this by POSTing to the `endpoint` URL:

https://api.gbif.org/v1/dataset/gbif-795b935c-8b88-4ed3-b30c-3473bfc3548f/endpoint

POST
```javascript
{
  "type":"DWC_ARCHIVE",
  "url":"https://github.com/rdmpage/gbif-795b935c-8b88-4ed3-b30c-3473bfc3548f/releases/latest/download/dwca.zip"
}
```

## Step 4

Wait for GBIF to index the data. After a few minutes the data should start appearing in GBIF maps and searches.

## Step 5 Edit and update

If the data needs to be tweaked, edit the data, put the new archive where it can be harvested (i.e., the endpoint) and ask GBIF to crawl it again.

https://api.gbif.org/v1/dataset/<UUID>/crawl

POST

Response

```HTTP/1.1 204 No Content```




