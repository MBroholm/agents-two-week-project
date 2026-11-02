Search the local movie collection by plot, themes, mood,
title, or a natural-language description.

Every call must include a non-empty question string.
Example input:
{"question": "movie about an astronaut stranded on Mars"}

Never call this workflow with {}.

Returns movie IDs, titles, years, plots, sources, and similarity
scores. Results are approximate matches; inspect the returned
plots to check whether they satisfy the request.