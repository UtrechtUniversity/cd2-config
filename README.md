# CD² CKAN config

This extension contains configurations and template files for the CD² CKAN portal.
The use of this extension is depended on several other extensions as described in the requirements section. 

This plugin is installed and configured by the [cd2-ansible script](https://github.com/UtrechtUniversity/cd2-ansible)

## Requirements
This extension has been developed and tested with CKAN version 2.9.*

This extension requires the following other extension to be installed and activated:

| CKAN extension        | Plugin   |
| ---------------       | ------------- |
| ckanext-scheming      | scheming_datasets |
| ckanext-scheming      | scheming_groups |
| ckanext-scheming      | scheming_organizations |
| ckanext-msl_ckan_util | msl_ckan |
| ckanext-msl_ckan_util | msl_custom_facets |
| ckanext-msl_ckan_util | msl_repeating_fields |


## Installation

**TODO:** Add any additional install steps to the list below.
   For example installing any non-Python dependencies or adding any required
   config settings.

To install ckanext-msl_ckan:

1. Activate your CKAN virtual environment, for example:

     . /usr/lib/ckan/default/bin/activate

2. Clone the source and install it on the virtualenv

            git clone https://git.science.uu.nl/epos-msl/epos-msl.git
            cd ckanext-msl_ckan
            pip install -e .
            pip install -r requirements.txt

3. Add `msl_ckan` to the `ckan.plugins` setting in your CKAN
   config file (by default the config file is located at
   `/etc/ckan/default/ckan.ini`).

4. Restart CKAN.


[AGPL](https://www.gnu.org/licenses/agpl-3.0.en.html)
