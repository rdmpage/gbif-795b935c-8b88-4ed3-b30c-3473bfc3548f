# A DNA barcode library for mangrove gastropods and crabs of Hong Kong and the Greater Bay Area reveals an unexpected faunal diversity associated with the intertidal forests of Southern China

## Step 1 Create dataset on GBIF

Create a dataset on GBIF using registry API. The **owningOrganizationKey** is the publisher UUID that you see in the link to the publisher page: https://www.gbif.org/publisher/92f51af1-e917-49bc-a8ed-014ed3a77bec. You also need a secret **installationKey** provided by GBIF, and you also need to authenticate the call using your GBIF portal username and password.

http://api.gbif.org/v1/dataset

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


