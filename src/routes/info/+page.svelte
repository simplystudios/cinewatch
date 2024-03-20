<script>
    import Layout from "../+layout.svelte";
    let id = window.location.pathname;
    console.log(id)
    let api = "https://consumetmuyi.vercel.app/movies/flixhq/info?id="
</script>
<Layout/>
