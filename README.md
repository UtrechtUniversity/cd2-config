# CD² CKAN config

This extension contains configuration, template files and images for the CD² CKAN portal.
The use of this extension is dependent on several other extensions as described in the requirements section. 

This plugin is installed and configured by the [cd2-ansible script](https://github.com/UtrechtUniversity/cd2-ansible)

## Requirements
This extension has been developed and tested with CKAN versions 2.9.* and 2.11.0

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

To install ckanext-cd2_config

1. Activate your CKAN virtual environment, for example:

     . /usr/lib/ckan/default/bin/activate

2. Clone the source and install it on the virtualenv

            git clone https://github.com/UtrechtUniversity/cd2-config.git
            cd ckanext-cd2_config
            pip install -e .
            pip install -r requirements.txt

3. Add `cd2_config` to the `ckan.plugins` setting in your CKAN
   config file (by default the config file is located at
   `/etc/ckan/default/ckan.ini`).

4. Restart CKAN.


[AGPL](https://www.gnu.org/licenses/agpl-3.0.en.html)
