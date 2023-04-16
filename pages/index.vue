<script>
export default {
    mounted() {
        if (process.client) {
            window.addEventListener('scroll', (event) => {
                this.lastKnownScrollPosition = window.scrollY;
                if (!this.ticking) {
                    window.requestAnimationFrame(() => {
                        this.handleScroll(this.lastKnownScrollPosition);
                        this.ticking = false;
                    });
                }
                this.ticking = true;
            });
        }

        this.stageReady = true;
    },
    unmounted() {
        window.removeEventListener('scroll', this.handleScroll);
    },
    data() {
        const appConfig = useAppConfig();
        var card1Panel;
        var nameTitleChn;
        var stageReady = ref(false);

        if (process.client) {
            card1Panel = document.querySelector('#card1Panel');
            nameTitleChn = document.querySelector('#nameTitleChn');
        }
        const animationCtx = reactive({
            progressList: [0],
            currentCard: 0,
            progress: 0.0,
            card1Portait: {
                transform: 0.0,
                shadowPos: 0,
                opacity: 1.0,
            },
            card1: {
                nameTitle: {
                    transform: 0.0,
                },
                briefOpacity: 0.0,
                emailOpacity: 0.0,
            }

        });
        var scrollFieldList;
        if (process.client) {
            scrollFieldList = document.querySelectorAll('.scroll-field');

        }

        const cubic_bezier_generator = (x1, y1, x2, y2, epsilon) => {

            var curveX = function (t) {
                var v = 1 - t;
                return 3 * v * v * t * x1 + 3 * v * t * t * x2 + t * t * t;
            };

            var curveY = function (t) {
                var v = 1 - t;
                return 3 * v * v * t * y1 + 3 * v * t * t * y2 + t * t * t;
            };

            var derivativeCurveX = function (t) {
                var v = 1 - t;
                return 3 * (2 * (t - 1) * t + v * v) * x1 + 3 * (- t * t * t + 2 * v * t) * x2;
            };

            return function (t) {

                var x = t, t0, t1, t2, x2, d2, i;

                // First try a few iterations of Newton's method -- normally very fast.
                for (t2 = x, i = 0; i < 8; i++) {
                    x2 = curveX(t2) - x;
                    if (Math.abs(x2) < epsilon) return curveY(t2);
                    d2 = derivativeCurveX(t2);
                    if (Math.abs(d2) < 1e-6) break;
                    t2 = t2 - x2 / d2;
                }

                t0 = 0, t1 = 1, t2 = x;

                if (t2 < t0) return curveY(t0);
                if (t2 > t1) return curveY(t1);

                // // Fallback to the bisection method for reliability.
                // while (t0 < t1) {
                //     x2 = curveX(t2);
                //     if (Math.abs(x2 - x) < epsilon) return curveY(t2);
                //     if (x > x2) t0 = t2;
                //     else t1 = t2;
                //     t2 = (t1 - t0) * .5 + t0;
                // }

                // Failure
                return curveY(t2);

            }
        };
        const ease_in_out = cubic_bezier_generator(0.42, 0.0, 0.58, 1.0, 0.05);
        const trisector = [0.0, 0.33, 0.67, 1.0];
        const quadrisector = [0.00, 0.25, 0.50, 0.75, 1.00];
        return {
            appConfig,
            lastKnownScrollPosition: 0,
            ticking: false,
            animationCtx,
            ease_in_out,
            card1Panel,
            nameTitleChn,
            stageReady,
            trisector,
            quadrisector,
            scrollFieldList,
        }
    },
    computed: {
        card1NameTitleTransform() {
            if (process.client) {
                var rect2 = this.card1Panel.getBoundingClientRect();
                var delta = rect2.left - window.innerWidth / 2 + (this.nameTitleChn.offsetWidth) / 2 + 40;
                return (this.ease_in_out(this.getInterpolation(this.animationCtx.progressList[0], this.quadrisector[0], this.quadrisector[1])) - 1) * delta;
            }
        },
        card1PortraitTransform() {
            if (process.client) {
                return (this.ease_in_out(this.getInterpolation(this.animationCtx.progressList[0], this.quadrisector[0], this.quadrisector[1])) - 1) * 3;
            }
        },
        card1PortraitShadowPos() {
            if (process.client) {
                return Math.round(this.getInterpolation(this.animationCtx.progressList[0], this.quadrisector[0], this.quadrisector[1]) * 100);
            } else {
                return 0;
            }
        },
        card1PortraitOpacity() {
            if (process.client) {
                return 1 - this.getInterpolation(this.animationCtx.progressList[0], this.quadrisector[1], this.quadrisector[2]);
            }
        },
        card1BriefOpacity() {
            if (process.client) {
                return this.getInterpolation(this.animationCtx.progressList[0], this.quadrisector[2], this.quadrisector[3]);
            } else {
                return 0;
            }
        },
        card1EmailOpacity() {
            if (process.client) {
                return this.getInterpolation(this.animationCtx.progressList[0], this.quadrisector[2], this.quadrisector[3]);
            } else {
                return 0;
            }
        },
        group1TitleTransform() {
            if (process.client) {
                return -this.getInterpolation(this.animationCtx.progressList[1], 1.0, 1.4) * 500 +
                    (1 - this.getInterpolation(this.animationCtx.progressList[1], -0.8, -0.5)) * 500;
            } else {
                return 0;
            }
        },
        group1Card1Transform() {
            if (process.client) {
                return -this.getInterpolation(this.animationCtx.progressList[1], 1.1, 1.5) * 500 +
                    (1 - this.getInterpolation(this.animationCtx.progressList[1], -0.6, -0.3)) * 500;
            } else {
                return 0;
            }
        },
        group1Card2Transform() {
            if (process.client) {
                return -this.getInterpolation(this.animationCtx.progressList[1], 1.3, 1.7) * 500 +
                    (1 - this.getInterpolation(this.animationCtx.progressList[1], -0.5, -0.2)) * 500;
            } else {
                return 0;
            }
        },
        group1Card3Transform() {
            if (process.client) {
                return -this.getInterpolation(this.animationCtx.progressList[1], 1.5, 1.9) * 500 +
                    (1 - this.getInterpolation(this.animationCtx.progressList[1], -0.4, -0.1)) * 500;
            } else {
                return 0;
            }
        },
        group1ImageOffset() {
            if (process.client) {
                return this.getInterpolation(this.animationCtx.progressList[1], -0.6, 1.9) * 10 + 45;
            }
            return 300;
        },
        card2Rotate() {
            if (process.client) {
                return this.getInterpolation(this.animationCtx.progressList[2], 0.2, 0.8) * 180;
            }
        },
    },
    methods: {
        handleScroll(pos) {
            var blockId = 0;
            while (blockId < this.scrollFieldList.length) {
                if (process.client) {
                    var rect = this.scrollFieldList[blockId].getBoundingClientRect();
                    this.animationCtx.progressList[blockId] = -rect.top / (rect.bottom - window.innerHeight - rect.top);
                    blockId++;
                } else {
                    if (blockId == 0) {
                        this.animationCtx.progressList[blockId] = 0.0;
                    } else {
                        this.animationCtx.progressList[blockId] = -1.0;
                    }
                }
            }
            return;
        },
        getInterpolation(progress, start, end) {
            return Math.min(Math.max((progress - start) / (end - start), 0.0), 1.0);
        },
    }
}
</script>

<template>
    <PageBlock>
        <SectionScroll>
            <CardWrap>
                <SectionCard class="bg-slate-100 -z-10 min-h-[600px]">
                    <SectionCardSide class="z-20 relative bg-blue-50 pr-[2px]"
                        :style="{ transform: `translateX(${card1PortraitTransform}rem)` }">
                        <img src="/img/CasualPhoto.JPG" class="object-cover h-full w-full rounded-[50px]" />
                        <div class="w-full h-full bg-gradient-to-r from-transparent to-slate-100 absolute" :style="{
                            '--tw-gradient-to-position': `${card1PortraitShadowPos}%`,
                                opacity: card1PortraitOpacity
                        }">
                        </div>
                    </SectionCardSide>
                    <SectionCardPanel id="card1Panel" class="z-30 justify-between">
                        <div></div>
                        <div class="">
                            <div id="nameBlock" :class="{ 'text-slate-100': !stageReady }"
                                :style="{ transform: `translateX(${card1NameTitleTransform}px)` }">
                                <div class="flex flex-nowrap">
                                    <div id="nameTitleChn" class="inline">
                                        <div class="text-[120px] mr-12 inline-flex">
                                            韩
                                        </div>
                                        <div class="text-[120px] inline-flex">
                                            兴华
                                        </div>
                                    </div>
                                </div>
                                <div class="flex flex-nowrap">
                                    <div class="inline">
                                        <div class="text-[60px] mr-12 inline-flex">
                                            Han,
                                        </div>
                                        <div class="text-[60px] inline-flex">
                                            Xinghua
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <p class="text-slate-800 text-[30px]" :style="{ opacity: card1BriefOpacity }">Full Stack, CG
                                Enthusiast and Optimist of Life</p>
                        </div>

                        <div class="flex-col flex text-[25px]" :style="{ opacity: card1EmailOpacity }">
                            <span>
                                Vlairrrrr@outlook.com
                            </span>
                            <span>
                                XinghHan@seas.upenn.edu
                            </span>
                        </div>
                    </SectionCardPanel>
                </SectionCard>
            </CardWrap>
        </SectionScroll>
        <SectionScroll>
            <CardWrap>
                <VerticalAlign>
                    <div class="flex-none h-[120px]">
                        <SectionCard class="pl-10" :style="{ transform: `translateY(${group1TitleTransform}px)` }">
                            <div class="text-[60px]">
                                My Projects
                            </div>
                        </SectionCard>
                    </div>
                    <HorizontalAlign>
                        <ImageCard :image-offset="group1ImageOffset" object-id="group1Card1" bg-image="/img/QuadDemo.png"
                            :style="{ transform: `translateY(${group1Card1Transform}px)` }">
                            Quadrilateral Meshing
                        </ImageCard>
                        <ImageCard :image-offset="group1ImageOffset" object-id="group1Card2" bg-image="/img/Broadcast.png"
                            :style="{ transform: `translateY(${group1Card2Transform}px)` }">
                            Campus Broadcast Platform
                        </ImageCard>
                        <ImageCard :image-offset="group1ImageOffset" object-id="group1Card3" bg-image="/img/English.svg"
                            :style="{ transform: `translateY(${group1Card3Transform}px)` }">
                            English Learning
                        </ImageCard>
                    </HorizontalAlign>
                </VerticalAlign>
            </CardWrap>
        </SectionScroll>
        <SectionScroll>
            <CardWrap>
                <VerticalAlign>
                    <div class="flex-none h-[120px]">
                        <SectionCard class="pl-10">
                            <div class="text-[60px]">
                                Education
                            </div>
                        </SectionCard>
                    </div>
                    <HorizontalAlign>
                        <ImageCard object-id="group1Card1" class="min-h-0" bg-image="/img/DLUTLogo.svg">
                            Dalian University of Technology
                        </ImageCard>
                        <ImageCard object-id="group1Card2" bg-image="/img/UPENN.svg">
                            University of Pennsylvania
                        </ImageCard>
                    </HorizontalAlign>
                </VerticalAlign>
            </CardWrap>
        </SectionScroll>
    </PageBlock>
</template>

<style></style>