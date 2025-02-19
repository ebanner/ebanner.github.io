---
layout: post
title:  "Garbage Collection"
---

Stuff piles up and gets stale. I don't want to maintain a file that I have to
find every time I want to append something to it.

So instead of viewing my stuff as a todo list that I need to get through, I view
it as a stream and clear it out on a regular basis.

Here's a video demo of how I do this with [YouTube videos](https://github.com/ebanner/archive-youtube-playlist):

<iframe width="560" height="315" src="https://www.youtube.com/embed/7sDBBOyIEOA" frameborder="0" allowfullscreen></iframe>

I also do it with [email](https://github.com/ebanner/gmail-auto-archive-daily).

I took original inspriation from the [Obsidian Daily Note](https://help.obsidian.md/Plugins/Daily+notes) feature.

# Project Links

- ![GitHub](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png){:width="18" style="vertical-align: -3px;"} [Archive YouTube Playlist](https://github.com/ebanner/archive-youtube-playlist)
- ![GitHub](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png){:width="18" style="vertical-align: -3px;"} [Gmail Auto Archive Daily](https://github.com/ebanner/gmail-auto-archive-daily)

<script>
  async function fetchPreview(url, container) {
    try {
      let response = await fetch(`https://api.linkpreview.net/?key=YOUR_API_KEY&q=${url}`);
      let data = await response.json();
      container.innerHTML = `
        <div class="link-preview">
          <img src="${data.image}" class="link-preview-img">
          <div class="link-preview-content">
            <a href="${url}" target="_blank"><strong>${data.title}</strong></a>
            <p>${data.description}</p>
          </div>
        </div>
      `;
    } catch (error) {
      container.innerHTML = `<a href="${url}" target="_blank">${url}</a>`;
    }
  }

  document.addEventListener("DOMContentLoaded", () => {
    document.querySelectorAll(".unfurl").forEach(link => {
      fetchPreview(link.dataset.url, link);
    });
  });
</script>

<div class="unfurl" data-url="https://example.com"></div>
