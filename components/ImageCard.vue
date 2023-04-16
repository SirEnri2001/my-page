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
            var linkBox = document.getElementById(this.$props.objectId);
            var animationBox = linkBox.cloneNode(true);
            var linkBoxRect = linkBox.getBoundingClientRect();
            document.body.appendChild(animationBox);
            animationBox.classList.add('z-40');
            animationBox.classList.remove('transition-all');
            animationBox.classList.remove('duration-1000');
            animationBox.classList.remove('ease-in-out');
            animationBox.classList.remove('group');
            animationBox.querySelector('.hoverbox').classList.remove('group-hover:opacity-100');
            animationBox.querySelector('.hoverbox').classList.remove('opacity-0');
            animationBox.style.width = linkBox.offsetWidth.toString()+'px';
            animationBox.style.height = linkBox.offsetHeight.toString()+'px';
            animationBox.style.position = 'fixed';
            animationBox.style.top= linkBoxRect.top.toString()+'px';
            animationBox.style.left = linkBoxRect.left.toString()+'px';
            console.log(linkBoxRect);
            console.log(animationBox.getBoundingClientRect());
            animationBox.classList.add('transition-all');
            animationBox.classList.add('duration-[1000ms]');
            animationBox.classList.add('ease-out');

            // Perform animation
            animationBox.style.top = '0';
            animationBox.style.left= '0';
            animationBox.style.width = '100vw';
            animationBox.style.height = '100vh';
            animationBox.style['border-radius'] = '0';
        }
    }
}
</script>

<template>
    <SectionCard :id="objectId" class="relative group" @click="cardClick" style="flex: 1">
        <img v-if="hasImage" :src="bgImage" alt="" class="object-cover w-full h-full"
            :style="{ 'object-position': `right ${imageOffset}% top 50%` }" >
        <div class="hoverbox ease-in-out duration-300 transition-all
                    flex items-center justify-center absolute h-full w-full
                    group-hover:opacity-100 opacity-0 backdrop-blur-[16px]">
            <div class="text-[75px] text-white justify-center items-center p-10 transition-all duration-300">
                <slot />
            </div>
        </div>
    </SectionCard>
</template>