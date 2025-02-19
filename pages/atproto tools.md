- tl;dr link: https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/m/fork/p/9
  collapsed:: true
	- the document opens in [fiddle mode](https://support.getgrist.com/glossary/#fiddle-mode) - you can make any changes you like
	- UI tips
		- right click on any column header to sort and filter by that column
		- use the big green button to add additional widgets such as charts or grouped tables
		- [tutorial template](https://templates.getgrist.com/doc/woXtXUBmiN5T)
		- https://support.getgrist.com/keyboard-shortcuts/
- Basically, we try collect, process and present software from [various awesome-atproto lists](https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/p/7) around the web.
  id:: 677672f7-5e29-4db1-a373-b6d3fed4549a
	- prior art: there are already a number of similar projects. The difference is that we put the data into an easily extensible sqlite db with an excel-like frontend (it still supports full sql queries tho), we use only open-source, self-hostable software to do it, and we don't sell ad space.
		- some of the other projects:
		  https://growbluesky.com/
		  https://blueskystarterpack.com/
		  https://bskyinfo.com/
		  https://blueskydirectory.com/
	- [db schema]([[schema]])
		- Sites
		  id:: 67767469-542f-48c0-b2a5-188b3fea1232
			- [Crawl each source](https://github.com/atproto-tools/atproto-tools-scripts/tree/main/f/data_sources)
				- made a [python module](https://github.com/atproto-tools/atproto-tools-scripts/blob/main/f/main/Collector.py) to provide an interface for automated processing
					- however i am a programming noob and just yoloed writing it, so it might not be great quality code :( feedback on the code is welcome too!
				- TODO add more sources! contributions and PRs very welcome.
				- TODO [contact](https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/p/7#a1.s19.r3.c684) all the data source owners and ask for forgiveness/permission
				- preserve categorization
				  collapsed:: true
					- TODO still a deal of work to be done of [unifying](https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/p/19) all this metadata, for example several sources have analogous tags for client apps.
		- Repos
		  id:: 67767430-00f2-4a86-81b0-84828fbfc695
			- [fetch basic info about the repos](https://github.com/atproto-tools/atproto-tools-scripts/blob/main/f/main/get_repos_data.py) with graphql, try to add homepage, author contact info, alternate URLs to prevent linkrot
			- TODO this is pretty rich data, we have access to all the code/commits/issues/etc from graphql. ideas?
			  collapsed:: true
				- track fork relationships
				- more details about licenses
				- aggregate more data about commits (frequency, # of contriubtors etc)
				- crawl the github lists for the more promising "related topics". probably use the search endpoint?
		- Authors
		  id:: 67767acd-45f5-41d7-b260-b2ca44dc3dd8
			- added directly or when crawling another data source.
				- [fetch basic profile info](https://github.com/atproto-tools/atproto-tools-scripts/blob/main/f/main/get_authors_data.py), unless user has `!no-unauthenticated` set
				- TODO [contact](https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/p/5#a1.s15.r1.c685) people to ask for forgiveness/permission to display their work
			-
- relevant docs for contributing
	- [windmill](https://www.windmill.dev/docs/intro)
	  collapsed:: true
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
	  collapsed:: true
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
	- manipulating the tables
	  collapsed:: true
		- grist
			- https://support.getgrist.com/api/
			- https://docs.getgrist.com/apiconsole
			- https://pygrister.readthedocs.io/en/stable/intro.html
- tasks for further development:
	- transform and prettify presentation
	  collapsed:: true
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
	  collapsed:: true
		- template https://templates.getgrist.com/dKztiPYamcCp/Crowdsourced-List/p/1
		- https://support.getgrist.com/access-rules/
- mindset:
  collapsed:: true
	- ![image.png](../assets/image_1735320252579_0.png) 
	  source: https://x.com/gf_256/status/1514131084702797827