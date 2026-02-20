Invoke the `chairman` agent to produce a complete, publish-ready blog post.

Pass the following user-supplied information to the chairman agent:

- **Topic:** $ARGUMENTS
- **Target audience:** ask the user if not already specified
- **Desired length:** ask the user if not already specified (default 800–1200 words)
- **Angle or special constraints:** ask the user if not already specified

If $ARGUMENTS is empty, ask the user for the topic before proceeding.

The chairman agent will coordinate the researcher, composer, fact-checker, and editor agents automatically, looping until the post is publish-ready, then return the final post and a Chairman's summary.
