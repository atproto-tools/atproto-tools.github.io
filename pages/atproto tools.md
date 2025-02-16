- link: https://atproto-tools.getgrist.com/t6bKvzR97jxB/main-list/m/fork/p/9
	- link is to a forkable version aka [fiddle mode](https://support.getgrist.com/glossary/#fiddle-mode) - you can fork or download the db from the green share button in the top left
- how it works
	- [db schema]([[schema]])
	  id:: 677672f7-5e29-4db1-a373-b6d3fed4549a
		- Sites
		  id:: 67767469-542f-48c0-b2a5-188b3fea1232
			- primary key URL of service/project homepage
		- Repos
		  id:: 67767430-00f2-4a86-81b0-84828fbfc695
			- primary key source code repo
				- we try to autodetect github and put them here in addition to on the Sites
		- Authors
		  id:: 67767acd-45f5-41d7-b260-b2ca44dc3dd8
			- primary key DID
	- [windmill](https://www.windmill.dev/docs/intro)
		- [local dev setup](https://www.windmill.dev/docs/advanced/local_development#develop-locally)
			- https://www.windmill.dev/docs/advanced/cli/installation
			- launch config:
			  collapsed:: true
				- locally add `__init__.py` to all project folders for project module includes. don't forget to add `- "**/__init__.py"` to excludes in wmill.yaml
				- add to launch.json launch config:
				  ```json
				  "env": {
				    "BASE_INTERNAL_URL": "https://app.windmill.dev",
				    "WM_TOKEN": "your token",
				    "WM_WORKSPACE": "atproto-tools-scripts",
				    "PYTHONPATH": "${workspaceFolder}"
				  }
				  ```
	- libraries for getting data
		- https://requests.readthedocs.io/en/stable/user/quickstart/
		- html
			- https://www.crummy.com/software/BeautifulSoup/bs4/doc/
				- js equivalent? https://github.com/taoqf/node-html-parser
				- https://scrapfly.io/web-scraping-tools/css-xpath-tester
		- markdown
			- https://mistune.lepture.com/en/latest/guide.html
		- graphql
			- https://graphql.org/learn/
			- https://docs.github.com/en/graphql/overview/explorer
			- [Get info about several repositories from Github Graphql API in a single call - Stack Overflow](https://stackoverflow.com/a/77549291/592606)
			-
			- https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/filtering-and-searching-issues-and-pull-requests#using-search-to-filter-issues-and-pull-requests
				- grabbing data from rss feeds of prs might be useful because they have more info about the author
			- https://docs.github.com/en/graphql/guides/using-pagination-in-the-graphql-api
			- https://docs.github.com/en/graphql/overview/rate-limits-and-node-limits-for-the-graphql-api
			- https://gql.readthedocs.io/en/stable/usage/
		- bsky api
			- https://public.api.bsky.app/xrpc/com.atproto.identity.resolveHandle?handle=<handle>
			- https://public.api.bsky.app/xrpc/app.bsky.actor.getProfile?actor=<did>
			- https://public.api.bsky.app/xrpc/app.bsky.actor.getProfiles?actors=<did>&actors=<did> (limit 25)
	- updating
		- grist
			- https://support.getgrist.com/api/
			- https://docs.getgrist.com/apiconsole
			- https://pygrister.readthedocs.io/en/stable/intro.html
	- transform and prettify presentation
		- possible ideas:
			- for formulas https://support.getgrist.com/functions/#record
			- alt presentation
				- https://support.getgrist.com/summary-tables/
					- https://community.getgrist.com/t/summary-table-with-dynamic-columns/1604/2
						- https://public.getgrist.com/2g1mnjbiUXCd/Cross-tabulation-table/p/4
						- https://public.getgrist.com/71GRe6EuiRtm/Combined-Tables/m/fork
						- https://community.getgrist.com/t/how-to-combine-two-tables-into-single-one/257/8
				- https://community.getgrist.com/t/lookuprecords-where-column-value-in-multiple-value/1656/2
				- https://support.getgrist.com/widget-custom/
			- load into an alternate db?
				- https://github.com/gristlabs/grist-core/issues/45#issuecomment-897678034
				- https://github.com/gristlabs/grist-core/issues/195
	- access control for everyone
		- template https://templates.getgrist.com/dKztiPYamcCp/Crowdsourced-List/p/1
		- https://support.getgrist.com/access-rules/
- mindset:
  collapsed:: true
	- ![image.png](../assets/image_1735320252579_0.png) 
	  source: https://x.com/gf_256/status/1514131084702797827