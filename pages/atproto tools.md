- tl;dr link: https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/m/fork/p/9
	- the link opens in [fiddle mode](https://support.getgrist.com/glossary/#fiddle-mode) - you can make any changes you like
	- UI tips:
		- right click on any column header to sort and filter by that column
		- use the big green button to add additional widgets such as charts or grouped tables
		- [tutorial template](https://templates.getgrist.com/doc/woXtXUBmiN5T)
		- https://support.getgrist.com/keyboard-shortcuts/
- Basically, we attempt to process and present links collected from [various awesome-atproto lists](https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/p/7) around the web.
  id:: 677672f7-5e29-4db1-a373-b6d3fed4549a
	- prior art: there are already a number of similar projects
		- The difference is
			- we put the data into [grist](https://github.com/gristlabs/grist-core/), a sqlite db with an excel-like frontend (still supports full sql queries through an api)
			- we use only open-source, self-hostable software, so anyone can fork the whole thing at any time
			  collapsed:: true
				- TODO unfortunately not actually self-hosted yet
			- and we don't sell ad space
		- some of the other projects:
		  https://growbluesky.com/
		  https://blueskystarterpack.com/
		  https://bskyinfo.com/
		  https://blueskydirectory.com/
	- [db schema]([[schema]])
		- Sites
		  id:: 67767469-542f-48c0-b2a5-188b3fea1232
			- [Use scripts to crawl our sources](https://github.com/atproto-tools/atproto-tools-scripts/tree/main/f/data_sources)
				- preserve categorization
					- TODO still a deal of work to be done of [unifying](https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/p/19) all this metadata, for example several sources have analogous tags for client apps.
				- [python module](https://github.com/atproto-tools/atproto-tools-scripts/blob/main/f/main/Collector.py) to provide an interface for automated processing
					- however i am a programming noob and just made it up as i went along, so it is likely not great quality code. feedback/prs welcome
				- TODO add more sources! contributions very welcome
				- TODO [contact](https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/p/7#a1.s19.r3.c684) the data source owners and ask for forgiveness/permission/feedback
		- Repos
		  id:: 67767430-00f2-4a86-81b0-84828fbfc695
			- [fetch basic info about the repos](https://github.com/atproto-tools/atproto-tools-scripts/blob/main/f/main/get_repos_data.py) with graphql, try to add homepage, author contact info, alternate URLs to prevent linkrot
			- TODO this is pretty rich data, we have access to all the code/commits/issues/etc from graphql. possible ideas:
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
		- The crawler scripts and the submission form are run from [windmill]()
- relevant docs
	- [windmill](https://www.windmill.dev/docs/intro)
	  collapsed:: true
		- [local dev setup](https://www.windmill.dev/docs/advanced/local_development#develop-locally)
			- https://www.windmill.dev/docs/advanced/cli/installation
			- windmill local setup:
			  collapsed:: true
				- add `__init__.py` to all project folders for project module includes. don't forget to add `- "**/__init__.py"` to excludes in wmill.yaml
				- add to launch config:
				  ```json
				  "env": {
				    "BASE_INTERNAL_URL": "https://app.windmill.dev",
				    "WM_TOKEN": "your_token",
				    "WM_WORKSPACE": "atproto-tools-scripts",
				    "PYTHONPATH": "${workspaceFolder}"
				  }
				  ```
	- getting data
	  collapsed:: true
		- https://requests.readthedocs.io/en/stable/user/quickstart/
		- html
		  collapsed:: true
			- https://www.crummy.com/software/BeautifulSoup/bs4/doc/
			  collapsed:: true
				- js equivalent? https://github.com/taoqf/node-html-parser
				- https://scrapfly.io/web-scraping-tools/css-xpath-tester
		- markdown
		  collapsed:: true
			- https://mistune.lepture.com/en/latest/guide.html
		- graphql
		  collapsed:: true
			- https://graphql.org/learn/
			- https://docs.github.com/en/graphql/overview/explorer
			- [Get info about several repositories from Github Graphql API in a single call - Stack Overflow](https://stackoverflow.com/a/77549291/592606)
			- https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/filtering-and-searching-issues-and-pull-requests#using-search-to-filter-issues-and-pull-requests
			  collapsed:: true
				- grabbing data from rss feeds of prs might be useful because they have more info about the author
			- https://docs.github.com/en/graphql/overview/rate-limits-and-node-limits-for-the-graphql-api
			- https://gql.readthedocs.io/en/stable/usage/
		- bsky api format:
		  collapsed:: true
			- https://public.api.bsky.app/xrpc/com.atproto.identity.resolveHandle?handle=<handle>
			- https://public.api.bsky.app/xrpc/app.bsky.actor.getProfiles?actors=<did>&actors=<did> (limit 25)
	- working with tables
	  collapsed:: true
		- grist
			- https://support.getgrist.com/api/
			- https://docs.getgrist.com/apiconsole
			- https://pygrister.readthedocs.io/en/stable/intro.html
- tasks for further development:
	- TODO transform and prettify presentation
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
	- TODO access control for crowdsourced editing
		- template https://templates.getgrist.com/dKztiPYamcCp/Crowdsourced-List/p/1
		- https://support.getgrist.com/access-rules/
- mindset:
  collapsed:: true
	- ![image.png](../assets/image_1735320252579_0.png) 
	  source: https://x.com/gf_256/status/1514131084702797827