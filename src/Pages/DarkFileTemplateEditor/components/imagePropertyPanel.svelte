<script>
  import InfoIcon from "../svg/infoIcon.svelte"
  import Link from "../svg/link.svelte"
  import PositonLock from "../svg/positonLock.svelte"
  import Tooltip from "../shared/tooltip.svelte"
  import Mark from "../svg/mark.svelte"
  import { fade, fly } from "svelte/transition"

  export let imageUploadedSection = false
  export let imageUploaded = false
  export let togglePosition = "DYNAMIC"

  let dargAndDrop = false
  let toggleButton = false
  let blobUrl,
    File,
    a,
    KB = 0,
    imgHeight = 0,
    imgWidth = 0

  let formData
  const uploadImage = () => {
    let image = document.getElementById("formImage")
    formData = new FormData(image)
    console.log([...formData])
    let datum = [...formData][0]
    File = datum[1]
    KB = Math.floor(File.size / 1000).toFixed(1)
    blobUrl = URL.createObjectURL(File)
    console.log(blobUrl)
    imageUploaded = !imageUploaded
    dargAndDrop = !dargAndDrop
    a = File.name
    console.log(a)
    setTimeout(() => {
      imgHeight = document.getElementById("uploadImage").naturalHeight
      imgWidth = document.getElementById("uploadImage").naturalWidth
    }, 200)
  }
  const displayUploadSection = () => {
    toggleButton = !toggleButton
    imageUploaded = false
    if (toggleButton) {
      dargAndDrop = true
      togglePosition = "STATIC"
    } else {
      dargAndDrop = false
      togglePosition = "DYNAMIC"
    }
  }
  const RemoveImage = () => {
    imageUploaded = !imageUploaded
    dargAndDrop = true
  }
</script>

{#if imageUploadedSection}
  <div class="bg-secondary left-0 top-14 flex h-auto min-w-[330px] max-w-[330px] flex-col border-b border-black pt-4 pb-4" in:fly={{ y: -200, duration: 1500 }} out:fly={{ y: -200, duration: 2000 }}>
    <div class="flex ">
      <div class="w-full flex-col items-center px-4">
        <div class=" group relative flex items-center rounded-md">
          <span class="labelSpan text-gray-400">NAME</span>
          <input name="field_name" class="inputValue" type="text" placeholder="" />
        </div>

        <!-- toggle button -->
        <div class="relative mt-3 flex h-9 w-full items-center gap-2 rounded-md bg-transparent px-2">
          <div class="focus:ring-primary_blue rounded-md focus:outline-none focus:ring-2">
            <div class="group/edit relative">
              <div class="group-hover/edit:block ml-1 hidden"><Tooltip tooltip="Info" top="true" /></div>
              <InfoIcon />
            </div>
          </div>
          <h1 class="text-sm font-bold text-gray-400">{togglePosition}</h1>
          <button on:click={displayUploadSection} class:justify-end={toggleButton} class="group/item bg-heading text-secondaryGray relative ml-auto mr-1 flex h-3 w-12 items-center rounded-full focus:outline-none">
            <button class="bg-primary_blue relative h-6 w-6 rounded-full text-xs text-white focus:outline-none " />
          </button>
        </div>

        <!-- image upload -->
        {#if dargAndDrop}
          <div class="mt-3" in:fly={{ y: -200, duration: 1500 }} out:fly={{ y: -200, duration: 1500 }}>
            <form enctype="multipart/form-data" id="formImage" accept="image">
              <label class="focus:outline-noneborder-gray-300 bg-secondary flex h-28 w-full cursor-pointer appearance-none justify-center rounded-md border border-dotted px-4 transition">
                <span class="flex items-center space-x-2">
                  <svg xmlns="http://www.w3.org/2000/svg" class="text-heading h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12" />
                  </svg>
                  <span class="text-secondaryGray font-medium">
                    Drop files to Attach, or
                    <span class="text-primary_blue underline">browse</span>
                  </span>
                </span>
                <input on:input={uploadImage} type="file" name="file_upload" class="hidden" />
              </label>
            </form>
          </div>
        {/if}

        <!-- image preview -->
        {#if imageUploaded}
          <div class="mt-3 items-center" in:fly={{ y: -200, duration: 600 }} out:fly={{ y: -200, duration: 600 }}>
            <div class=" bg-primary flex  rounded-md px-1.5 py-1">
              <div class="flex h-24 w-full flex-row items-stretch gap-2">
                <div class="w-4/12">
                  <img src={blobUrl} alt="QRCode" id="uploadImage" class="h-full w-full cursor-pointer rounded-md" />
                </div>
                <div class="group/item relative flex w-8/12 items-start gap-2 rounded-md">
                  <div class="flex flex-col gap-1">
                    <h3 class="text-heading h-5 w-32 overflow-hidden text-sm font-bold">
                      <div class="hidden group-hover:block"><Tooltip tooltip={a} top={true} /></div>
                      {a}
                    </h3>

                    <span class="text-heading text-xs  font-bold">{imgWidth}px X {imgHeight}px</span>
                    <div class="text-heading text-xs font-bold">{KB}KB</div>
                  </div>
                  <button on:click={RemoveImage} class="group/edit relative right-0 top-0 ml-8 flex items-start justify-end rounded-md hover:text-red-600 hover:ring-2 focus:outline-none focus:ring-2 active:bg-red-600 active:text-white">
                    <div class="group-hover/edit:block hidden"><Tooltip tooltip="Back" top={true} /></div>
                    <Mark />
                  </button>
                </div>
              </div>
            </div>
          </div>
        {/if}
        <div class="text-secondaryGray mt-3 ml-2 flex text-sm">Positions</div>
        <!-- position lock -->
        <div class=" mt-2 flex items-center justify-between gap-3 text-base">
          <div class="bg-primary group  relative mx-auto flex w-2/5 items-center justify-between rounded-md focus:outline-none">
            <div class="labelSpan text-primary">X</div>
            <input name="field_name" class="inputField" type="number" min="0" placeholder="" />
          </div>
          <div class="bg-primary focus:bg-secondary focus:ring-primary_blue group relative mx-auto flex w-2/5 items-center justify-between rounded-md focus:outline-none focus:ring-2">
            <div class="labelSpan text-gray-400">Y</div>
            <input name="field_name" class="inputField" type="number" min="0" placeholder="" />
          </div>
          <button class="lockPositionBtn group relative">
            <div class=" z-10 hidden group-hover:block">
              <Tooltip tooltip="Lock Position" top={true} />
            </div>
            <PositonLock />
          </button>
        </div>
        <!-- image width and height -->
        <div class="text-secondaryGray mt-3 ml-2 text-sm">Dimensions</div>
        <div class=" mt-2 flex items-center gap-3 text-base">
          <div class="bg-primary group relative mx-auto flex w-2/5 items-center justify-between rounded-md focus:outline-none ">
            <div class="labelSpan text-gray-400">W</div>
            <input name="field_name" class="inputField" type="number" min="0" placeholder="" />
          </div>

          <div class="group relative mx-auto flex w-2/5 items-center justify-between rounded-md focus:outline-none ">
            <div class="labelSpan  text-gray-400">H</div>
            <input name="field_name" class="inputField" type="number" min="0" placeholder="" />
          </div>
          <button class="lockPositionBtn group relative">
            <div class="flex text-3xl">
              <div class=" hidden group-hover:block ">
                <Tooltip tooltip="Maintain Aspect Ratio" top={true} />
              </div>
              <Link />
            </div>
          </button>
        </div>
      </div>
    </div>
  </div>
{/if}

<style lang="postcss" global>
  .labelSpan {
    @apply absolute rounded-md border-none px-2 py-2 text-sm font-bold text-gray-400 outline-none focus:ring-1 focus:ring-primary_blue group-focus-within:text-primary_blue;
  }
  .lockPositionBtn {
    @apply flex h-8 w-8 items-center justify-center rounded-md hover:ring-1 hover:ring-primary_blue active:bg-primary_blue;
  }
  .inputField {
    @apply w-full rounded-md bg-secondary px-2 py-2 pl-8 text-sm text-white focus:bg-secondary focus:outline-none  focus:ring-1 focus:ring-primary_blue;
  }
  .inputValue {
    @apply w-full rounded-md bg-secondary px-4 py-2 pl-16 text-sm font-bold text-white focus:bg-secondary focus:outline-none focus:ring-1 focus:ring-primary_blue;
  }
</style>
