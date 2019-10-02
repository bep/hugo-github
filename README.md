# Hugo GitHub

Some small (proof of concept) Hugo components talking to GitHub.



## repo

Include it as a Hugo Component in your `module` config in `config.toml`:

```
[[module.imports]]
    path="github.com/bep/hugo-github/repo"
```

Then you can do something like this:


```
{{ $repo := "gohugoio/hugo" }}
{{ $githubRepo := partialCached "github/repo" $repo $repo }}

{{ range $k, $v :=  $githubRepo }}
    Key: {{ $k }} Value: {{ $v }}
{{ end }}
```

Which will print out information about the given repo, including `stargazers_count`.

The last `$repo` argument above is the cache key. The above partial uses `getJSON` under the hood, so the result is also cached to disk, see https://gohugo.io/getting-started/configuration/#configure-file-caches for how to configure.

Running the above without any access token will soon get you over the GitHub rate limit. You can get by this by setting a `GITHUB_TOKEN` as an OS environment variable. Note that this token will be sent to GitHub as a request attribute (because of a limitation in Hugo), which is considered less secure than sending as a HTTP header, as it may show up in server logs.