<script>
export default {
    data() {
        var hasImage = false;
        if (this.$props.bgImage && this.$props.bgImage != '') {
            hasImage = true;
        }
        return {
            hasImage
        }

    },
    props: {
        bgImage: String,
        imageOffset: Number,
        objectId: String,
    },
    methods:{
        cardClick(event) {
            var proj1 = document.getElementById(this.$props.objectId);
            var parent = proj1.parentElement
            var parentRect = parent.getBoundingClientRect();
            proj1.classList.add('z-40');
            proj1.classList.remove('transition-all');
            proj1.classList.remove('duration-1000');
            proj1.classList.remove('ease-in-out');
            proj1.style.position = 'fixed';
            proj1.style.top= parentRect.top.toString()+'px';
            proj1.style.left = parentRect.left.toString()+'px';
            proj1.style.width = parent.offsetWidth+'px';
            proj1.style.height = parent.offsetHeight+'px';
            proj1.classList.add('transition-all');
            proj1.classList.add('duration-300');
            proj1.classList.add('ease-out');
            proj1.style.top = '0';
            proj1.style.left= '0';
            proj1.style.width = '100vw';
            proj1.style.height = '100vh';
            proj1.style['border-radius'] = '0';
        }
    }
}
</script>

<template>
    <SectionCard :id="objectId" class="relative group" @click="cardClick" style="flex: 1">
        <img v-if="hasImage" :src="bgImage" alt="" class="object-cover w-full h-full"
            :style="{ 'object-position': `right ${imageOffset}% top 50%` }" >
        <div class="ease-in-out duration-300 transition-all
                    flex items-center justify-center absolute h-full w-full
                    group-hover:opacity-100 opacity-0 backdrop-blur-[16px]">
            <div class="text-[75px] text-white justify-center items-center p-10 transition-all duration-300">
                <slot />
            </div>
        </div>
    </SectionCard>
</template>