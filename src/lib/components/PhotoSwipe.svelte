<script>
import { onMount } from "svelte";
import PhotoSwipeLightbox from 'photoswipe/lightbox';
import 'photoswipe/style.css';
  import { urlFor } from "$lib/sanity";
export let galleryID;
export let images = [];
export let width = '29%'
 let imageDimensions = []
onMount(() => {
  prepareGallery()
    let lightbox = new PhotoSwipeLightbox({
      gallery: '#' + galleryID,
      children: 'a',
      pswpModule: () => import('photoswipe'),
    });
    lightbox.init();})

function prepareGallery(){
       
        images.forEach((img, index) => {
        const image = new Image();
        image.src = urlFor(img).url();
        image.onload = () => {
            
            if(window.innerWidth < 800){
                let width = window.innerWidth
                let height = (window.innerWidth)* image.naturalHeight / image.naturalWidth
                imageDimensions[index] = { width: width,height: height};
            }else{
                if(image.naturalWidth>window.innerWidth*.85){
                let width = (window.innerWidth*.85 )<1200?window.innerWidth*.85 :1200
                let height = (window.innerWidth*.85 )* image.naturalHeight / image.naturalWidth
                imageDimensions[index] = { width: width,height: height};
                }else{
                    imageDimensions[index] = {
                    width: image.naturalWidth,
                    height: image.naturalHeight,
                };
            }
            }
            
        };
      
        });
       
     
    }
    
  </script>
    <div class="pswp-gallery " id={galleryID}>
    {#each images as image,i}
    <a href={urlFor(image).url()} 
          target="_blank" data-pswp-width='{imageDimensions[i]?.width}' data-pswp-height='{imageDimensions[i]?.height}'
          rel="noreferrer">
          <img src={urlFor(image).url()} alt="image{i}" style=";width:{i==0?'50%':i==1?'32%':width}" class="m-sm-2 m-1"/>
          </a>
    {/each}
</div>



<style>
img{object-fit: contain; margin-bottom: 1rem;box-shadow: rgba(14, 63, 126, 0.04) 0px 0px 0px 1px, rgba(42, 51, 69, 0.04) 0px 1px 1px -0.5px, rgba(42, 51, 70, 0.04) 0px 3px 3px -1.5px, rgba(42, 51, 70, 0.04) 0px 6px 6px -3px, rgba(14, 63, 126, 0.04) 0px 12px 12px -6px, rgba(14, 63, 126, 0.04) 0px 24px 24px -12px;}
    a{cursor: pointer;}
</style>

