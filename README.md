docker run -it -v `pwd`'/frontend/src/config.js:/app/frontend/src/config.js' -v `pwd`'/report/config.json:/app/report/config.json' -v "$HOME/.aws/:/root/.aws/" cloudefficiency

# EC2 c-type instance cost/waste interactive reports by team and manager.
Three stages to produce EC2 c-type instance cost/waste interactive reports.
ldap data + instance data -> user hierarchy and instance data -> static html/js isomorphic React pages.

## First two stages in python
`python3 -m report.allocate_efficiency`

### preparing raw data
1. gets ldap data for user hierarchy, and dl lists.
2. gets rightsizing recommendations for instances from cloudability.

caches data to:
- ldap_entries.pickle
- dl_owners.pickle
- rightsizing_cache.json

### attributing ownership
3. assigns cost and waste to each individual
4. aggregate costs and wastes up reporting chain

outputs:
- allInstances.js jsonp file
- allUsers.js jsonp file

`mv all*.js to frontend/src/`

# generating reports
node ./dist/generate_files.js

cwd has `./output/*.html`
`./public/index.css` has styling
`./public/bundle.js` has js and user/instance data

# Configuration files
`logo.svg` in `frontend/public/logo.svg`
`config.js` in `frontend/src/config.js`
```
const awsmobile = {
    'aws_app_analytics': 'enable',
    'aws_mobile_analytics_app_id': <string>,
    'aws_mobile_analytics_app_region': <string>,
    'aws_project_id': <string>,
    'aws_project_name': <string>,
    'aws_project_region': <string>,
    'aws_resource_name_prefix': <string>
}
const VPLIST = [<string>...]

export { awsmobile, VPLIST };

```
`config.json` in `report/config.json`
```
{
  "ldap_url": <string>,
  "ldap_port": <number>,
  "ldap_basedn": <string>,
  "ldap_binddn": <string>,
  "ldap_bindpw": <string>,
  "ldab_dl_basedn": <string>,
  "ldap_dl_search_filter_template": <string>,
  "ldap_search_filter": <string>,
  "ldap_searchreq_attrlist": [<string>...],
  "ldap_users_cache_file": <filename>,
  "ldap_cache_time": <number>,
  "cloudability_api_key": <string>,
  "VP_LIST": [<string>...]
}

```


## Analytics Events

Root Render (document.referrer, page)
{
    name: 'appRender'
});


Analytics.updateEndpoint({
    page: ...,
	referrer: ...,
})

click CPE logo
{
    name: 'click',
    attributes: {
    	target: 'logo'
    },
});

click issues
{
    name: 'click',
    attributes: {
    	target: 'issues'
    },
});

click teammember
{
    name: 'click',
    attributes: {
    	target: 'teammember',
    	targetUser: 'name'
    },
});

click manager
{
    name: 'click',
    attributes: {
    	target: 'manager',
    	targetUser: 'name'
    },
});

click owner
{
    name: 'click',
    attributes: {
    	target: 'instanceOwner',
    	targetUser: 'name',
    	targetInstance: 'id'
    },
});

click ownerVp
{
    name: 'click',
    attributes: {
    	target: 'pwmerVP',
    	targetUser: 'name',
    	targetInstance: 'id'
    },
});

hover tooltip
{
    name: 'tooltip',
    attributes: {
    	target: 'leadership' | 'user' | 'id' | 'type' | 'suggest' | 'waste' | 'cost' | 'owner' | 'vpOwner' | 'attribution', 'moneyExplanation,
    },
});
{
    name: 'tooltip',
    attributes: {
    	target: 'attribution',
    	targetUser: 'name'
    },
});
