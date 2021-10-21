# ckanext-msl_ckan

This extension contains configurations and templates for the EPOS MSL CKAN portal.
The use of this extension is depended on several other extensions as described in the requirements section. Reusable 
functionality has been placed within the msl_ckan_util extension.

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

**TODO:** Add links to extension repos

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

## SOLR changes

Depending on how SOLR was installed combined with CKAN a schema.xml supplied with the CKAN installation has 
been used. These changes assume the CKAN supplied schema.xml have been used. The following additions should be 
made to the schema.xml.

Add to `<fields>` definitions:

      <!-- MSL custom fields for indexing and web services -->
      <field name="msl_hidden_text" type="text" indexed="true" stored="false" multiValued="true"/>
    
      <!-- coming from IPackageController msl_search.MslIndexRepeatedFieldsPlugin::before(index) -->
      <field name="msl_material" type="string" indexed="true" stored="true" multiValued="true"/>
      <field name="msl_rock_measured_property" type="string" indexed="true" stored="true" multiValued="true"/>

And to the bottom list with `copyField` definitions add:

      <!-- customizations MSL-->
      <copyField source="msl_material" dest="text"/>
      <copyField source="msl_hidden_text" dest="text"/>

Within the `solrconfig.xml` make sure that the `<str name="q.op">` setting is set to AND for the select request handler:

      <requestHandler name="/select" class="solr.SearchHandler">
    <!-- default values for query parameters can be specified, these
         will be overridden by parameters in the request
      -->
    <lst name="defaults">
      <str name="echoParams">explicit</str>
      <int name="rows">10</int>
      <str name="mm">1</str>
      <str name="q.op">AND</str> <----
      ...

## Config settings

This extension includes several configuration files that are used by other extension required by this project. To 
make the correct links to the other extensions/plugins the following lines should be added to the `ckan.ini`.

### Load plugins

`ckan.plugins` in the `ckan.ini` should contain the following plugins:

      msl_ckan
      scheming_datasets
      scheming_groups
      scheming_organizations 
      msl_custom_facets
      msl_repeating_fields

Make sure to keep the above order of plugin declaration in the `ckan.ini`. The order of plugin loading determines the 
order of execution of hooks and usage of templates.

### plugin specific settings

To use the schemas as included within this extension by the scheming plugin the following lines should be added to the 
`ckan.ini` file:

      CKAN.SCHEMING.DATASET_SCHEMAS=ckanext.msl_ckan:schemas/datasets/ckan_dataset.yaml ckanext.msl_ckan:schemas/datasets/rock_physics.yml ckanext.msl_ckan:schemas/datasets/labs.json
      CKAN.SCHEMING.GROUP_SCHEMAS=ckanext.msl_ckan:schemas/groups/custom_group_msl_subdomain.json
      CKAN.SCHEMING.ORGANIZATION_SCHEMAS=ckanext.msl_ckan:schemas/organizations/custom_org_institute.json

To use the included facet configuration:

      CKAN.MSLFACETS.DATASET_CONFIG=ckanext.msl_ckan:config/facets.json

To use the included index fields configuration:

      CKAN.MSLINDEXFIELDS.FIELD_CONFIG=ckanext.msl_ckan:config/msl_index_fields.json

## Developer installation

To install ckanext-msl_ckan for development, activate your CKAN virtualenv and
do:

    git clone https://git.science.uu.nl/epos-msl/epos-msl.git
    cd ckanext-msl_ckan
    python setup.py develop
    pip install -r dev-requirements.txt


## Tests

To run the tests, do:

    pytest --ckan-ini=test.ini


## Releasing a new version of ckanext-msl_ckan

If ckanext-msl_ckan should be available on PyPI you can follow these steps to publish a new version:

1. Update the version number in the `setup.py` file. See [PEP 440](http://legacy.python.org/dev/peps/pep-0440/#public-version-identifiers) for how to choose version numbers.

2. Make sure you have the latest version of necessary packages:

    pip install --upgrade setuptools wheel twine

3. Create a source and binary distributions of the new version:

       python setup.py sdist bdist_wheel && twine check dist/*

   Fix any errors you get.

4. Upload the source distribution to PyPI:

       twine upload dist/*

5. Commit any outstanding changes:

       git commit -a
       git push

6. Tag the new release of the project on GitHub with the version number from
   the `setup.py` file. For example if the version number in `setup.py` is
   0.0.1 then do:

       git tag 0.0.1
       git push --tags

## License

[AGPL](https://www.gnu.org/licenses/agpl-3.0.en.html)
