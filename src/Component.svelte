<script>
  import { getContext } from "svelte"

  export let data;
  export let encodingProtection;

  const { styleable, Provider  } = getContext("sdk")
  const component = getContext("component")

  //https://stackoverflow.com/questions/12168909/blob-from-dataurl
  function dataURItoBlob(dataURI) {
    // convert base64 to raw binary data held in a string
    // doesn't handle URLEncoded DataURIs - see SO answer #6850276 for code that does this
    var byteString = atob(dataURI.split(",")[1]);

    // separate out the mime component
    var mimeString = dataURI.split(",")[0].split(":")[1].split(";")[0];

    // write the bytes of the string to an ArrayBuffer
    var ab = new ArrayBuffer(byteString.length);

    // create a view into the buffer
    var ia = new Uint8Array(ab);

    // set the bytes of the buffer to the correct values
    for (var i = 0; i < byteString.length; i++) {
      ia[i] = byteString.charCodeAt(i);
    }

    // write the ArrayBuffer to a blob, and you're done
    var blob = new Blob([ab], { type: mimeString });
    return blob;
  }
  //blob to blob url
  function dataURIToBlobURL(dataURI) {
    return URL.createObjectURL(dataURItoBlob(dataURI));
  }

  let dataURL = [];
  let bolbURL = [];
  let name = [];
  let type = [];
  let size = [];
    $: dataURL = data? JSON.parse((encodingProtection? data.slice(1,-1): data)).map((e) => e.link): [];
    $: bolbURL = data? JSON.parse((encodingProtection? data.slice(1,-1): data)).map((e) => dataURIToBlobURL(e.link)): [];
    $: name = data? JSON.parse((encodingProtection? data.slice(1,-1): data)).map((e) => e.name): [];
    $: type = data? JSON.parse((encodingProtection? data.slice(1,-1): data)).map((e) => e.type): [];
    $: size = data? JSON.parse((encodingProtection? data.slice(1,-1): data)).map((e) => e.size): [];
</script>

<div use:styleable={$component.styles}>
  {#each dataURL as url, i}
        <Provider data={{dataURL:url, blobURL: bolbURL[i], name: name[i], type: type[i], size: size[i]}}>
        <slot />
      </Provider>
      {/each}
</div>
