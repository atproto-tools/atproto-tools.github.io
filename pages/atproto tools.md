- main link: https://atproto-tools.getgrist.com/p2SiVPSGqbi8/m/fork
	- ui tips:
		- right click on any column header to sort and filter
		  collapsed:: true
			- can sort by multiple columns at once
		- https://support.getgrist.com/keyboard-shortcuts/
		- [tutorial doc](https://templates.getgrist.com/doc/woXtXUBmiN5T)
	- the link is in [fiddle mode](https://support.getgrist.com/glossary/#fiddle-mode)- it creates your own copy of the db that you can freely edit/save
	  collapsed:: true
		- besides the usual sql/csv exports, grist dbs also be exported as a `.grist` file (including all metadata) and imported into another install of grist. might be handy if excess popularity ever becomes a concern.
	- [submission form](https://app.windmill.dev/a/atproto-tools-scripts/submit) to add a record directly to the main db
		- reach out on bsky https://aeshna-cyanea.bsky.social if you want to get full edit access to the main doc or to otherwise contribute (help v much needed)
- basically, this is an attempt to help monitor/enable discovery of the atproto ecosystem by processing links collected from [various awesome-atproto/bsky lists](https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/p/7) from around the web.
  id:: 677672f7-5e29-4db1-a373-b6d3fed4549a
	- prior art: there are already a number of similar projects
		- what makes this one different:
			- we put the data into [grist](https://github.com/gristlabs/grist-core/), a sqlite db with an excel-like frontend that is [extensible with html/js](https://support.getgrist.com/widget-custom/) (and supports [sql queries](https://support.getgrist.com/api/#tag/sql))
			- we use software (grist and windmill) that is open-source and self-hostable
				- as is the [code for collecting/processing the data](https://github.com/atproto-tools/atproto-tools-scripts/)
				- TODO unfortunately not actually self-hosted yet
			- we do not sell ad space, ever, guaranteed, i hate ads
		- some projects that may be bigger and more user friendly but don't fit the above description:
		  https://growbluesky.com/
		  https://blueskystarterpack.com/
		  https://bskyinfo.com/
		  https://blueskydirectory.com/
	- structure/process
		- [db schema]([[schema]])
			- Sites
			  id:: 67767469-542f-48c0-b2a5-188b3fea1232
				- [Use scripts to crawl our sources](https://github.com/atproto-tools/atproto-tools-scripts/tree/main/f/data_sources)
					- try to preserve categorization
						- TODO still a deal of work to be done of [unifying](https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/p/19) all this metadata, for example several sources have analogous tags for client apps.
					- [python module](https://github.com/atproto-tools/atproto-tools-scripts/blob/main/f/main/Collector.py) to provide an interface for automated processing
						- however i am a programming noob and just made it up as i went along, so it is likely not great quality code. feedback/prs welcome
					- TODO add more sources! contributions very welcome
					- TODO [contact](https://atproto-tools.getgrist.com/p2SiVPSGqbi8/main-list/p/7#a1.s19.r3.c684) the data source owners and ask for forgiveness/permission/feedback
					- TODO additional parsing of websites themselves
						- maybe automatically grab the page title from the html tags (title or meta/ogl tags).
						- flag for review (is site inactive?) based on the http resp code
				- tagging schema
					- Concept - what thing is the tool working with
						- Identity
							- DID
							- Handle
						- Lexicon
						- PDS
						  collapsed:: true
							- Record
							- Repo
						- Client - shorthand for `Record, Aggregate+Manipulate`, often also a `Resource`
						- Client mod - browser extensions, userscripts
						- Oauth
						- Sync
						  collapsed:: true
							- Relay
							- Jetstream
						- Appview
						- Labeler
						- Protocol-wide
					- Lexicon - with subcategories for record types
					  collapsed:: true
						- Bluesky
						  collapsed:: true
							- Post
							- Follow
							- Block
							- Like
							- List
							- Starter Pack
							- Feedgen
							- DM
						- Frontpage
						  collapsed:: true
							- Comment
							- Post
							- Vote
					- Type - what type of tool is it. meaning if e.g. an open source client also has an official hosted version, it's both a tool and a resource.
					  collapsed:: true
						- Resource - a resource or program meant to be used as-is.
						- Tool - a library or tool useful for creating other tools- i.e. can be conceivably included as a dependency in something.
					- Verb - what does our tool do with the thing.
					  collapsed:: true
						- Select - can work with many of this thing and switch between them.
						  collapsed:: true
						  For example, a client that supported several lexicons would be able to `Select Lexicon`, one with an account switcher can `Select Repo`
						- Aggregate - works with multiple existing records and combines them somehow
						- Manipulate - create or delete records
						  collapsed:: true
							- Batch Manipulate
						- Transform - edits or converts records
						  collapsed:: true
							- Batch Transform
					- Tags - other common tool attributes
					  collapsed:: true
						- Documentation - for when a project is documentation or has good documenation (subjective)
						- Bridging - resources that continuously bridge protocols
						- Migration - tools or resources that let you convert records between lexicons/protocols
						- Analytics
						  collapsed:: true
							- Small scale
							- Large scale (lots of repos)
							- Visualization
						- Complex/dev-oriented
					- Interop with
					  collapsed:: true
						- Twitter
						- Mastodon
						- Nostr
			- Repos
			  id:: 67767430-00f2-4a86-81b0-84828fbfc695
				- [fetch basic info about the repos](https://github.com/atproto-tools/atproto-tools-scripts/blob/main/f/main/get_repos_data.py) with graphql
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
		- The crawler scripts and the submission form are run on [windmill](http://windmill.dev/)
			- If you would like to contribute code, you can dm me on bsky [omniraptor.bsky.social](http://omniraptor.bsky.social) and i'll add you to the workspace. or just open a PR on the github
			- TODO the submission form could ideally have atproto authentication. so you sign in and get ownership over your own records
- relevant docs for contributing:
	- [windmill](https://www.windmill.dev/docs/intro)
		- [local dev setup](https://www.windmill.dev/docs/advanced/local_development#develop-locally)
		  collapsed:: true
			- https://www.windmill.dev/docs/advanced/cli/installation
			- windmill local setup:
			  collapsed:: true
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
	- working with grist
		- https://support.getgrist.com/api/
		- https://docs.getgrist.com/apiconsole
		- https://pygrister.readthedocs.io/en/stable/intro.html
		- https://support.getgrist.com/webhooks/#webhooks
		- self hosted version [apparently supports](https://github.com/gristlabs/grist-core/pull/588#pullrequestreview-1546296858) network requests from formulas?
- tasks for further development: #buildinpublic
	- an opt-out mechanism:
	  collapsed:: true
		- a special 'opted-out' column on Sites, Repos, Authors
			- when set, the (url, did) of that record is set to an encrypted version of itself
				- key is stored with other secrets like api tokens
				- encrypted records is excluded from all queries by default- add that to ATPTGrister
			- add handling when adding entries
				- author- abort adding anything by author- propagating to linked sites/repos
				- site- if adding from form, warn the submitter.
					- or not? idk, ask for design help
				- repo-
					- this one's difficult. probably just silently abort
				- all these would need significant refactoring, cuz we rely on the add functions returning a value
					- i guess now we see the benefits of functional programming lmao
	- transform and prettify presentation
		- come up with a logo for social media
		- possible ideas for the db:
			- #q so if i want to add a feature matrix for bsky clients, should i add it as a separate table? or as yet more columns in the main Sites table. hmm
			- [custom widgets](https://support.getgrist.com/widget-custom/)
				- mirror and directly render git repo READMEs in a widget
			- formula reference https://support.getgrist.com/functions/#record
			- load the data into an alternate db?
				- https://github.com/gristlabs/grist-core/issues/45#issuecomment-897678034
				- https://github.com/gristlabs/grist-core/issues/195
			- alt presentation
				- https://support.getgrist.com/summary-tables/
					- https://community.getgrist.com/t/summary-table-with-dynamic-columns/1604/2
						- https://public.getgrist.com/2g1mnjbiUXCd/Cross-tabulation-table/p/4
						- https://public.getgrist.com/71GRe6EuiRtm/Combined-Tables/m/fork
						- https://community.getgrist.com/t/how-to-combine-two-tables-into-single-one/257/8
				- https://community.getgrist.com/t/lookuprecords-where-column-value-in-multiple-value/1656/2
	- access control for better crowdsourced editing
		- basically add a secret column that tracks SessionIDs (from direct edits) and uuid from linkKeys (in the submission form) https://support.getgrist.com/access-rules/
			- however having secret data prevents forking. would also need to add automation to regularly copy the main doc to a separate version with stripped secret data, and let people copy from that. clunky!
				- would be fixed by https://github.com/gristlabs/grist-core/issues/791
		- template https://templates.getgrist.com/dKztiPYamcCp/Crowdsourced-List/p/1
- done tasks:
	- so what determines the default layout of a record card?
		- go to raw data > click card icon on a table
- common formulas/snippets:
  collapsed:: true
	- joining urls:
		- `" ".join((f"[{i[8:]}]({i})" if i.startswith("https://") else i) for i in <list of urls>)`
	- normalizing a url:
	  collapsed:: true
		- for grist formulas:
		  ```python
		  from urllib.parse import urlparse, urlunparse, parse_qsl, urlencode
		  parsed = urlparse($url)
		  #TODO more tracking params, maybe a library?
		  query = urlencode([
		      i for i in parse_qsl(parsed.query)
		      if not re.match('(?:utm_|fbclid|gclid|ref$).*', i[0])
		  ])
		  netloc = parsed.netloc.lower()
		  if netloc.startswith("www."):
		      netloc = netloc[4:]
		  path = re.match(r"(.*?)(/about)?/?$",parsed.path).group(1) #type: ignore
		  scheme = parsed.scheme
		  if scheme == "http":
		      scheme = "https"
		  return urlunparse(parsed._replace(netloc=netloc, path=path, scheme=scheme, query=query))
		  ```
		- standalone function:
		  ```python
		  def normalize(url: str) -> kf:
		      parsed = urlparse(url)
		      #TODO catch more tracking params, maybe look for a library?
		      query = urlencode([
		          i for i in parse_qsl(parsed.query)
		          if not re.match('(?:utm_|fbclid|gclid|ref$).*', i[0])
		      ])
		      netloc = parsed.netloc.lower()
		      netloc = netloc[4:] if netloc.startswith("www.") else netloc
		      path = re.match(r"(.*?)(/about)?/?$",parsed.path).group(1) #type: ignore
		      scheme = "https" if parsed.scheme == "http" else parsed.scheme
		      return urlunparse(parsed._replace(netloc=netloc, path=path, scheme=scheme, query=query)) #type: ignore
		  ```
- mindset:
  collapsed:: true
	- ![image.png](../assets/image_1735320252579_0.png) 
	  source: https://x.com/gf_256/status/1514131084702797827