# Hugo

## Installing Hugo (only if you want run the server localy)
Your password is mandatory to install Python Pygments.

`make install`

# Run Hugo Server (Only if you are not using a vagrant VM)

`make server-start`

# Writing a post

### With Hugo 

`hugo new dev/my-first-post.md`

The new file is located at `content/dev/my-first-post.md`

## Code highlighting

Hugo is using `Python Pygments` package for code highlighting so you can highlight your code with the dedicated tag like this:

```
{{< highlight php >}}
{{< /highlight >}}
```

## What about my medias

All media files linked to an article need to be stored into the static directory and more precisely into the static/images/posts (Group by year of publication)

To display a media into a post use Hugo shortcode `figure` like the following:

```
<p class="text-center">
    {{< figure src="/images/posts/2014/proxmox_partition_elao_1000.png" title="Partitionnement personnalisé" alt="Partitionnement-d-un-serveur-proxmox - Partitionnement personnalisé">}}
</p>
```
