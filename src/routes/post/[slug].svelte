<script context="module">
  export const load = async ({ page, fetch }) => {
    const res = await fetch(`/post/${page.params.slug}.json`);
    if (res.ok) {
      const post = await res.json();
      return {
        props: { post }
      };
    }
  };
</script>

<script>
  import { onMount } from 'svelte';

  export let post;

  function createTooltips() {
    let links = document.getElementsByClassName('footnote-link');

    let foot;
    let footHtml;
    let newDiv;
    let parent;

    for (let l of links) {
      parent = l.parentNode;
      foot = document.getElementById(`${l.id}-body`);

      footHtml = foot.innerHTML;

      newDiv = document.createElement('div');
      newDiv.innerHTML = footHtml;
      newDiv.className = 'footnote-tooltip';
      parent.insertBefore(newDiv, l.nextSibling);
    }
  }

  function showTooltips() {
    const widthPoint = 1314;
    const width = document.body.clientWidth;

    let tooltips = document.getElementsByClassName('footnote-tooltip');

    for (let t of tooltips) {
      t.style.display = width >= widthPoint ? 'block' : 'none';
    }

    let bodies = document.getElementsByClassName('footnote-body');

    for (let b of bodies) {
      b.style.display = width >= widthPoint ? 'none' : 'block';
    }
  }

  onMount(() => {
    createTooltips();
    showTooltips();
    window.addEventListener('resize', showTooltips);
  });
</script>

<svelte:head>
  <title>{post.title}</title>
</svelte:head>

<div class="content h-entry">
  <h1 class="p-name">{post.title}</h1>

  <div class="space-yo post-info">
    <div>
      <small>
        publié sur <span class="dt-published">{post.date}</span>
      </small>

      <small class="readtime">{post.readtime} min de lecture</small>
    </div>

    <div>
      <small class="fw-100">
        <a href="/post">&#9166; retour</a>
      </small>
    </div>
  </div>

  <div class="thumbnail-container">
    <img
      src="https://cdn.halcyonnouveau.xyz/blog/thumbnails/{post.thumbnail}?w=672&h=410"
      alt="thumbnail"
      class="thumbnail"
    />
  </div>

  <div class="e-content">
    {@html post.html}
  </div>

  <div class="info-line footer">
    <small>
      <a
        href="https://github.com/beanpuppy/justin.duch.me/edit/master/_posts/{post.date}-{post.slug}.md"
      >
        see a mistake? edit it here.
      </a>
    </small>

    <small>
      <a href="/post">&#9166; retour</a>
    </small>
  </div>
</div>

<style>
  .content .info-line {
    display: flex;
    justify-content: space-between;
  }

  .info-line a {
    text-decoration: none;
  }

  .thumbnail-container {
    min-height: 310px;
  }

  .p-name {
    margin: 0;
  }

  .content,
  .post-info {
    padding-bottom: 1em;
  }

  .footer {
    padding-top: 1em;
  }

  .content :global(pre) {
    padding: 0.5em;
    border-radius: 2px;
    overflow-x: auto;
  }

  .content :global(pre) :global(code) {
    max-width: 100%;
    overflow-x: auto;
  }

  .content :global(code.prettyprint) {
    color: #b9b9b9;
    background: #282828;
    background-color: rgba(41, 41, 41, 0.8);
  }

  .content :global(ul) {
    line-height: 1.5;
  }

  .content :global(li) {
    margin: 0 0 0.5em 0;
  }

  .content :global(blockquote) {
    background: #282828;
    border-left: 1em solid #484848;
    margin: 1.5em 1em;
    padding: 0.5em 1em;
    quotes: '\201C''\201D''\2018''\2019';
  }

  .content :global(blockquote:before) {
    content: open-quote;
    font-size: 4em;
    line-height: 0.1em;
    margin-right: 0.25em;
    vertical-align: -0.6em;
  }

  .content :global(blockquote:after) {
    content: close-quote;
    font-size: 1px;
  }

  .content :global(blockquote) :global(p) {
    display: block;
  }

  .content :global(img) {
    display: block;
    max-width: 100%;
    height: auto;
    margin-left: auto;
    margin-right: auto;
  }
  .content :global(iframe) {
    width: 100%;
  }

  .content :global(table) {
    border-collapse: collapse;
    width: 100%;
  }

  .content :global(th),
  .content :global(td) {
    text-align: left;
    padding: 0.8em;
  }

  .content :global(th) {
    border-bottom: 1px solid;
  }

  .content :global(tr):nth-child(even) {
    background-color: #282828;
  }

  .content :global(hr) {
    text-align: center;
    border: 0;
    border-top: 1px solid rgba(255, 255, 255, 0.2);
    margin-top: 3.8em;
  }

  .content :global(hr:after) {
    content: '§';
    display: inline-block;
    position: relative;
    top: -0.7em;
    font-size: 1.5em;
    padding: 0 0.25em;
    background: #191919;
  }

  .content :global(.footnote-tooltip),
  .content :global(.footnote-body) {
    font-size: smaller;
    background-color: #282828;
    padding: 0.5em 1em;
  }

  .content :global(.footnote-tooltip) {
    position: absolute;
    margin-left: 42em;
    margin-top: -2em;
    width: 30em;
  }

  .e-content :global(a:after) {
    position: relative;
    content: '\FEFF°';
    margin: 0 0.15em;
    top: -0.1em;
    color: #ec53dd;
    font-feature-settings: 'caps';
    font-variant-numeric: normal;
  }

  :global(sup) {
    font-size: 0.9rem;
  }

  .e-content :global(.footnote > sup) {
    font-weight: 100;
  }

  .e-content :global(.footnote-link:after),
  .e-content :global(.reference-link:after) {
    display: none;
  }

  .fw-100 {
    font-weight: 100;
  }

  @media screen and (max-width: 648px) {
    .content :global(blockquote) {
      margin: 0;
    }

    .thumbnail-container {
      min-height: 210px;
    }
  }
</style>
