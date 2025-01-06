- link: https://atproto-tools.getgrist.com/t6bKvzR97jxB/main-list/m/fork/p/9
	- link is to a forkable version aka [fiddle mode](https://support.getgrist.com/glossary/#fiddle-mode) - you can fork or download the db from the green share button in the top left
	- (pm me if you want to help contribute)
- tasks
	- db schema
	  id:: 677672f7-5e29-4db1-a373-b6d3fed4549a
	  collapsed:: true
		- Datasource # optional
		  id:: 677672fd-d514-4780-9a0c-ca7342a71a6f
			- URL = grist.Reference('Sites', reverse_of='URL') # two-way
			- Name = grist.Text()
			- Description = grist.Text()
			- Author = grist.Reference('Authors')
			- Tags = grist.ReferenceList('Datasource_Tags')
			- ...
		- Datasource_tags
			- Tag = grist.Text()
			- ...
		- Sites
		  id:: 67767469-542f-48c0-b2a5-188b3fea1232
			- URL = grist.Text()
			- Lexicons = grist.ReferenceList('Lexicons')
			- Skeet_Tools = grist.Reference('Datasource', reverse_of='URL')
			- Skeet_Tools_Name = grist.Text()
			- Skeet_Tools_Description = grist.Text()
			- Skeet_Tools_Tags = grist.ReferenceList('Skeet_Tools_Tags')
			- Skeet_Tools_Rating = grist.Numeric()
		- Repos
		  id:: 67767430-00f2-4a86-81b0-84828fbfc695
			- URL = grist.Text()
			- Last_Commit = grist.Date()
			- Open_Issues = grist.Int()
			- Owner = grist.Reference('Authors', reverse_of='Repos')
			- Open_PRs = grist.Int()
			- Stars = grist.Int()
			- Last_Release_Date = grist.Date()
			- Forks = grist.Int()
			- Sites = grist.Reference('Sites, reverse_of='Repos')
			  id:: 67767f18-833b-4669-a645-dc696b0e2725
		- Authors
		  id:: 67767acd-45f5-41d7-b260-b2ca44dc3dd8
		  collapsed:: true
			- VCS_Profiles = grist.ChoiceList()
			- DID = grist.Text()
			- Repos = grist.ReferenceList('Repos', reverse_of='Owner')
			- Organization = grist.Bool()
			- Bluesky_Name = grist.Text()
			- Bluesky_Handle = grist.Text()
			- Github_Social_Accounts = grist.Text()
			- Generic_Website = grist.Text()
	- getting data
		- normalize records
		  collapsed:: true
			- first iteration: grab everything, build sets/dicts of tags/authors/repos
				- schema for passing to next step
					- "records": entry_records, (list of db records)
						- record :
							- require: str # specifically url field
							- fields : str
								- name_field :
								- desc_field : str,
								- tags_field : ["L", *str],
								- rating_field : any] # but prefer 0-1 float for rating
					- "repos": repo_records (list of urls),
					- "authors": authors, (dict with str keys,or list) key is url, val is column
					  collapsed:: true
						- TODO clarify possible columns. for now:
							- DID (displayed as pure did, links to bsky.app)
							- bsky profile
							- other (website)
					- "table": table_name, (str)
					- "entry columns": {name_field, desc_field, tags_field, rating_field} # optional (both table and all individual columns)
		- https://pipedream.com/docs/
		- https://pygrister.readthedocs.io/en/stable/intro.html
		- https://requests.readthedocs.io/en/stable/user/quickstart/
			- https://docs.python-requests.org/en/latest/user/advanced/#session-objects
		- https://www.crummy.com/software/BeautifulSoup/bs4/doc/
			- js equivalent? https://github.com/taoqf/node-html-parser
		- https://github.com/miyuchina/mistletoe/blob/master/dev-guide.md
		- https://graphql.org/learn/
		- https://docs.github.com/en/graphql/overview/explorer
		- https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/filtering-and-searching-issues-and-pull-requests#using-search-to-filter-issues-and-pull-requests
			- grabbing data from rss feeds of prs might be useful because they have more info about the author
		- https://docs.github.com/en/graphql/guides/using-pagination-in-the-graphql-api
		- https://docs.github.com/en/graphql/overview/rate-limits-and-node-limits-for-the-graphql-api
		- https://public.api.bsky.app/xrpc/com.atproto.identity.resolveHandle?handle=
	- updating
		- order of ops per datasource:
		  id:: 67769af6-1b12-4df3-9d70-75c680b32eba
			- put datasource - get keys
			- put sites - write link to datasource - get site keys
			- if repos:
				- put repos - write link to datasource and sites
			- if authors:
				- put repos - write link to datasource and sites
			- if both authors and repos:
				- put repos - write link to datasource and sites - get repos
				- put authors - write link to datasource, sites, and repos
			- TODO low priority: make this more elegant, some kind of iteration instead of if clauses
		- order of ops per datasource (single table):
		  id:: 677957dd-04b1-42af-8b7e-982a17690f30
			- put sites- get site keys
			- if both authors and repos:
				- put repos - get repos
				- put authors - write link to datasource and repos
			- TODO low priority: make this more elegant, some kind of iteration instead of if clauses
		-
		- grist
			- https://support.getgrist.com/api/
			- https://docs.getgrist.com/apiconsole
			- https://support.getgrist.com/code/modules/GristData/#cellvalue
			- https://github.com/ben-pr-p/grist-js
	- transform and prettify presentation
	  collapsed:: true
		- possible ideas:
			- https://support.getgrist.com/widget-custom/
			- https://support.getgrist.com/summary-tables/
			- https://community.getgrist.com/t/summary-table-with-dynamic-columns/1604/2
				- https://public.getgrist.com/2g1mnjbiUXCd/Cross-tabulation-table/p/4
				- https://public.getgrist.com/71GRe6EuiRtm/Combined-Tables/m/fork
				- https://community.getgrist.com/t/how-to-combine-two-tables-into-single-one/257/8
			- https://community.getgrist.com/t/lookuprecords-where-column-value-in-multiple-value/1656/2
			- https://github.com/gristlabs/grist-core/issues/195
			- https://community.getgrist.com/t/many-to-any-relationships/650/4
				- https://github.com/gristlabs/grist-core/issues/45#issuecomment-897678034
	- access control for everyone
		- template https://templates.getgrist.com/doc/dKztiPYamcCp~nBCSFZ8tuCyUMQybsE37Ye~68125/p/1
		- https://support.getgrist.com/access-rules/
- mood:
  collapsed:: true
	- ![image.png](../assets/image_1735320252579_0.png) 
	  source: https://x.com/gf_256/status/1514131084702797827