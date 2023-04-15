<template>
    <SectionScroll>
            <CardWrap>
                <div class="w-full h-full relative" style="transform-style: preserve-3d;"
                    :style="{ transform: `perspective(1000px) rotateX(${card2Rotate}deg)` }">
                    <SectionCard style="backface-visibility:hidden;transform-style: preserve-3d;" class="absolute">
                        <SectionCardPanel>
                            <div>
                                <div class="flex flex-nowrap">
                                    <div class="inline">
                                        <div class="text-[120px] inline-flex">
                                            大连理工大学
                                        </div>
                                    </div>
                                </div>
                                <div class="flex flex-nowrap">
                                    <div class="inline">
                                        <div class="text-[40px] inline-flex">
                                            Dalian University of Technology
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </SectionCardPanel>
                        <SectionCardSide>
                            <img src="/img/DLUTLogo.svg" alt="" class="w-full">
                        </SectionCardSide>
                    </SectionCard>
                    <SectionCard
                        style="backface-visibility:hidden;transform-style: preserve-3d;transform: rotateX(-180deg);"
                        class="absolute">
                        <SectionCardPanel>
                            <div>
                                <div class="flex flex-nowrap">
                                    <div class="inline">
                                        <div class="text-[120px] inline-flex">
                                            University of Pennsylvania
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </SectionCardPanel>
                        <SectionCardSide class="p-16">
                            <img src="/img/UPENN.svg" alt="" class="w-full">
                        </SectionCardSide>
                    </SectionCard>
                </div>
            </CardWrap>
        </SectionScroll>
</template>