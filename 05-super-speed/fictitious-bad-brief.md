# Let's just add a "boost" button

We should ship a button in the console that lets handlers manually boost their
responder's ranking whenever they want. Engineering can wire it up to bump
the recent-acceptance score directly. This would be a great quick win and
handlers have been asking for more control.

While we're at it, we could also let handlers set their own timeout value
per responder, and maybe expose a few of the other routing weights too, so
they can tune things for their own roster. Might as well give quartermasters
a similar override in Supply while we're in there, since they've complained
about scheduling too.

This should be pretty quick to build and will make a lot of people happy.

Let's get this on the roadmap for next sprint.
