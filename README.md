# Jupyter notebook for accessing the Copernicus Land Monitoring Service - High Resolution Water, Snow & Ice (HR-WSI) ice products

## Description
This notebook (`HR-WSI-ice-webinar.ipynb`) is the companion to the HR-WSI **snow** webinar notebook, this time focused on the **ice** products of the HR-WSI portfolio: Water and Ice Cover (WIC S1, WIC S2, and the combined WIC S1+S2) and Ice Cover Duration (ICD). Data access mechanics (authentication, filtering, downloading via the CDSE OData API) are only briefly recapped here, since they were already covered in full in the snow webinar notebook — this notebook focuses instead on what the ice products show and how to work with them.

The notebook is organized into four short live sections and an appendix for self-study:
- **Section 1** — Define the area/season of interest and the OData query helper (fast recap of the query workflow; no download happens here)
- **Section 2** — Download a small ±2-week example window and visualize/compare WIC S1 (Sentinel-1, radar) vs. WIC S2 (Sentinel-2, optical), including their official legends
- **Section 3** — Filter observations using the QAFLAGS quality layer (bit-encoded, different between S1 and S2), reusing Section 2's example download
- **Section 4** — Application: estimate freeze-up and break-up dates for a lake. Once a lake is selected via EU-Hydro, the full season is downloaded scoped to that lake's own footprint, and WIC S1's ice fraction is filtered on QAFLAGS (temporary water, windy, grassland, high tree cover density, missing ancillary data) before building the S1/S2/combined ice-fraction time series
- **Appendix** (not presented live) — Ice Cover Duration (ICD) annual product, and a short note on the Aggregated Water and Ice Cover (AWIC) product

It can be run on the JupyterLab environment offered by the CDSE. To run it locally, follow the instructions below.

Visit the CLMS portal land.copernicus.eu for more details on the HR-WSI products (documentation, production or dissemination updates).

## Data used by this notebook
The HR-WSI products themselves are downloaded on the fly via the CDSE OData API, scoped to what each section actually needs rather than the whole season up front: Section 2/3 download a small ±2-week window around one example date, and Section 4 downloads the full season once — scoped to the bounding box of whichever lake is selected, computed programmatically from its EU-Hydro geometry, rather than a fixed AOI.

Section 4 also needs a copy of the **EU-Hydro** reference dataset to define a lake's boundary:
- Download it from https://land.copernicus.eu/en/products/eu-hydro/eu-hydro-river-network-database#download, either as a **single-basin zip** (covering just the river basin your lake is in) or as the **whole-Europe zip** (which bundles one zip per basin) — the notebook accepts either.
- Place it in an `auxiliary` folder next to the notebook (create the folder if it doesn't exist).
- If you're using the whole-Europe zip, set `EUHYDRO_BASIN_HINT` in Section 4 to a substring identifying your basin (e.g. `"kemi"`), so the notebook knows which of the bundled per-basin zips to open.
- The lake polygons live in the `InlandWater` layer of the downloaded geodatabase; lakes are identified by the `NAM` (name, not unique) and `LAKID` (unique ID) attributes — see the short EU-Hydro structure note in Section 4 for details.

Downloaded WIC/ICD products are extracted into a `products` folder, and EU-Hydro data into `auxiliary`, both created automatically next to the notebook — nothing but the notebook itself and `credentials.txt` needs to live loose in the working folder.

## Contact
Copernicus Land Monitoring Service service desk: https://land.copernicus.eu/en/contact-service-helpdesk

## Installation to run the notebook locally on Visual Studio Code
[Prerequiste] Install Conda (https://docs.conda.io/projects/conda/en/stable/user-guide/install/linux.html) 
Install Visual Studio Code (https://code.visualstudio.com/download) 

A Python environment is needed with the packages described in the _ypnb_env.yaml_ file:
```S
conda env create -f ypnb_env.yaml # to create a new Python environment with Conda
```
The environment is called _ypnb_env_. It has to be activated. 
```S
conda activate ypnb_env
```
The environment can be deactivated with `conda deactivate` and reactivated later with `conda activate ypnb_env`.

Run the Jupyter notebook `HR-WSI-ice-webinar.ipynb` in Visual Studio Code using the `ypnb_env` environment, or on CDSE JupyterLab using the "Geo Science" kernel — see the Prerequisites section at the top of the notebook itself for account setup, `credentials.txt`, and EU-Hydro placement.

## Legal notice about Copernicus Data
Access to data is based on a principle of full, open and free access as established by the Copernicus data and information policy Regulation (EU) No 1159/2013 of 12 July 2013. This regulation establishes registration and licensing conditions for GMES/Copernicus users and can be found here: http://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32013R1159.  

Free, full and open access to this data set is made on the conditions that:  
1. When distributing or communicating Copernicus dedicated data and Copernicus service information to the public, users shall inform the public of the source of that data and information.  
2. Users shall make sure not to convey the impression to the public that the user's activities are officially endorsed by the Union.  
3. Where that data or information has been adapted or modified, the user shall clearly state this.  
4. The data remain the sole property of the European Union. Any information and data produced in the framework of the action shall be the sole property of the European Union. Any communication and publication by the beneficiary shall acknowledge that the data were produced “with funding by the European Union”.  
