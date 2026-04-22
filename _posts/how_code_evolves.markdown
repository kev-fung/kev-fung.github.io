





for data scientists (non engineers) they usually will be applying basic understnading of Python into their framework. for example, they don't understand how S3 data in an orchestrator will be moved between the framework. e.g. we'll need to write our own function to handle data moving between tasks in a graph.


we'll also need to think about how teams build workflows over time.
what if a team because lack of skill set decided to build a flyte workflow and started to orchestrate everything within it? and because the team people didn't know how to do it?
-- must think how a team builds things and maintains them over time.
---- the point is to keep code as efficient, concise, yet expressive as possible.

a framework should be intuitive and handle the lack of engineering knowledge a team will have.

you need to realise that most newbies/data scientists only understand imperative programming (i.e. from python). This means for flyte, makes sense to keep it's graph building logic/ flow imperative rather than declarative.

now we'll also need to think about agents contributing to the workflows as well. Will they go crazy?

for agent specs:
-- place discovery hook functions in there for it to call e.g. a magic_func() capturing what it needed for discovery.
-- give spec on the style of programming, let it decide or you decide the code to look imperative, declarative, functional, do we want code patterns in it.




