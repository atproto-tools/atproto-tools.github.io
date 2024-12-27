- general protocol
	- This might be a dumb question but why can’t the relay and appview just keep the last 24 hours or week of content and drop the older signed events? Aren’t user feeds validated at ingestion and you could just check the signature instead of the merkle DAG no? Partial validation in SSB worked just fine. #relay #perf #bnewbold
	  https://bsky.app/profile/bnewbold.net/post/3ldwah3sg4s2d
- moderation tooling
	- Have there been any statements or analysis on why it's 20 labelers and not 200? 200 is a much more reasonable number imo #moderation #labelers #perf #jaz
	  collapsed:: true
	  https://discord.com/channels/1097580399187738645/1102623788799107092/1255758658198048828
		- scaling is the main answer here and need
		  every request you make when subscribing to 20 labelers triggers a lookup for labels from 20 potential issuers on every entity hydrated (every actor, post, list, etc.) in the API response
		  increasing that by an order of magnitude can blow up the amount of work done to serve requests significantly, I wouldn't rule out the limit increasing again (we've already done it once)
		  for now we utilize bloom filters to keep the cost of hydrating labels low (99% of the content you request doesn't have labels on it)
		  But those will eventually end up with lock contention if every entity hydrated requires making hundreds of lookups against the bloom filter 
		  for instance, loading your timeline hydrates something like 100+ entities
		  multiply that by 200 and you're doing 20,000 label lookups per timeline page
		  that's something we can optimize in the future if/when we need to though
		  we use ScyllaDB which is great but means we don't have table joins and doing a lookup that results in a "not found" case is generally slower than looking up something that exists in our experience
		  so being able to short-circuit that 99% case of a label not existing is very desirable
	- Reply gating is great but would it be too much overhead to have reply gating for individual replies as well as root posts? #moderation #perf #jaz
	  https://discord.com/channels/1097580399187738645/1098906064960880801/1210333025221738568
		- the implementation as it is is already pretty hacky as is and creates a data dependency (you should try to index reply gates before indexing replies). I don't see reply gating as being easily doable on non-root-posts tbh
		  And you do have control over what shows up directly under your post if you block the person who replies to you
		  Replying to a post 100 links down in a chain would require evaluating 100 reply gates in that case
		  It also creates some weird semantics around like
		  Replying to soemone's post but then preventing them from replying to your reply
		  or anyone in the thread above etc.
			- is it because they're meant to be changeable after posting? like if your post blows up too much
				- I don't think we allow that, it's more so that they stay after someone deletes a post
				  i.e. you can delete a post but people can technically still reply to it
				  but if there's still a threadgate sticking around, people can't violate the threadgate after the post is deleted
				  there are also probably some other reasons for it too that Paul and Danny would know better than I do