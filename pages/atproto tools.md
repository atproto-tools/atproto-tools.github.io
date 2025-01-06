- link: https://atproto-tools.getgrist.com/t6bKvzR97jxB/main-list/m/fork/p/9
	- link is to a forkable version aka [fiddle mode](https://support.getgrist.com/glossary/#fiddle-mode) - you can fork or download the db from the green share button in the top left
	-
- tasks
	- [db schema]([[triangle schema]])
	  id:: 677672f7-5e29-4db1-a373-b6d3fed4549a
		- Sites
		  id:: 67767469-542f-48c0-b2a5-188b3fea1232
			- primary key URL of service/project homepage
		- Repos
		  id:: 67767430-00f2-4a86-81b0-84828fbfc695
			- primary key source code repo
				- try to autodetect github and put them here in addition to on the Sites
		- Authors
		  id:: 67767acd-45f5-41d7-b260-b2ca44dc3dd8
			- primary key DID
	- getting data
		- https://pipedream.com/docs/
		- https://pygrister.readthedocs.io/en/stable/intro.html
		- https://requests.readthedocs.io/en/stable/user/quickstart/
		- html
			- https://www.crummy.com/software/BeautifulSoup/bs4/doc/
				- js equivalent? https://github.com/taoqf/node-html-parser
				- https://scrapfly.io/web-scraping-tools/css-xpath-tester
		- markdown
			- https://mistune.lepture.com/en/latest/guide.html
		- graphql
		  collapsed:: true
			- https://graphql.org/learn/
			- https://docs.github.com/en/graphql/overview/explorer
			- https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/filtering-and-searching-issues-and-pull-requests#using-search-to-filter-issues-and-pull-requests
				- grabbing data from rss feeds of prs might be useful because they have more info about the author
			- https://docs.github.com/en/graphql/guides/using-pagination-in-the-graphql-api
			- https://docs.github.com/en/graphql/overview/rate-limits-and-node-limits-for-the-graphql-api
		- bsky api
			- https://public.api.bsky.app/xrpc/com.atproto.identity.resolveHandle?handle=<handle>
			- https://public.api.bsky.app/xrpc/app.bsky.actor.getProfile?actor=<did>
		- output format
			- schema for passing to next step
			  ```python
			  return {
			          "records": entry_records, # {url: {columns}, ...} 
			          "repos": repos, # {url: [repo_urls], ...}
			          "authors": authors, # {url: [author_dids], ...}
			          "source": source_name, # str
			          "columns": [name_field, desc_field, tags_field, rating_field], # these are the default ones, all optional
			      }
			  ```
	- updating
		- order of ops per datasource (single table):
		  id:: 677957dd-04b1-42af-8b7e-982a17690f30
			- put sites- get site keys
			- if both authors and repos:
				- put repos - get repos
				- put authors - write link to datasource and repos
			- TODO low priority: make this more elegant, some kind of iteration instead of if clauses
		- grist
			- https://support.getgrist.com/api/
			- https://docs.getgrist.com/apiconsole
			- https://support.getgrist.com/code/modules/GristData/#cellvalue
			- https://github.com/ben-pr-p/grist-js
	- transform and prettify presentation
		- possible ideas:
			- for formulas https://support.getgrist.com/functions/#record
			- alt presentation
			  collapsed:: true
				- https://support.getgrist.com/summary-tables/
					- https://community.getgrist.com/t/summary-table-with-dynamic-columns/1604/2
						- https://public.getgrist.com/2g1mnjbiUXCd/Cross-tabulation-table/p/4
						- https://public.getgrist.com/71GRe6EuiRtm/Combined-Tables/m/fork
						- https://community.getgrist.com/t/how-to-combine-two-tables-into-single-one/257/8
				- https://community.getgrist.com/t/lookuprecords-where-column-value-in-multiple-value/1656/2
				- https://support.getgrist.com/widget-custom/
			- load into an alternate db?
			  collapsed:: true
				- https://github.com/gristlabs/grist-core/issues/45#issuecomment-897678034
				- https://github.com/gristlabs/grist-core/issues/195
	- access control for everyone
		- template https://templates.getgrist.com/doc/dKztiPYamcCp~nBCSFZ8tuCyUMQybsE37Ye~68125/p/1
		- https://support.getgrist.com/access-rules/
- mood:
  collapsed:: true
	- ![image.png](../assets/image_1735320252579_0.png) 
	  source: https://x.com/gf_256/status/1514131084702797827