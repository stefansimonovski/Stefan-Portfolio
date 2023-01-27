<template>
  <div class="flex flex-col gap-[2.5rem]">
    <div class="divCenter">
      <p
        id="landing_intro"
        ref="landingIntro"
        class="lg:text-xl text-md acrlyic w-auto text-center selector"
      ></p>
    </div>
    <section class="">
      <h1 class="text-4xl font-bold">Stefan Simonovski</h1>
      <h3 class="text-lg">Full stack web developer</h3>
    </section>
    <section class="pt-2">
      <h2
        class="lg:text-2xl text-lg font-bold underline underline-offset-8 decoration-2"
      >
        Bio
      </h2>
      <br />
      <p class="lg:text-lg text-sm text-justify leading-6">
        Professional full stack web developer with 3+ years experience in
        delivering professional looking and user-friendly websites from stratch.
        I have worked with a lot of businesses over the past 3 years. I have
        logical and practical solutions on how to approach your job on designing
        the high performance web applications using latest technologies.
      </p>
      <br />
      <p class="lg:text-lg text-sm text-justify leading-6">
        I am an enthusiastic, self-motivated, reliable, responsible and hard
        working person. I am a mature team worker and adaptable to all
        challenging situations. I am able to work well both in a team
        environment as well as using own initiative. I am able to work well
        under pressure and adhere to strict deadlines.
      </p>
    </section>
    <section class="">
      <h2
        class="lg:text-2xl text-lg font-bold underline underline-offset-8 decoration-2"
      >
        Journey
      </h2>
      <br />
      <div class="flex flex-col gap-2">
        <p class="lg:text-lg text-sm text-justify">
          <strong>2017:</strong>
          &nbsp; Started journey on programming as a hobby
        </p>
        <p class="lg:text-lg text-sm text-justify">
          <strong>2018:</strong>
          Certificate for responsible web site in Photoshop, HTML5 and CSS3 in
          Semos Education - School of computer science
        </p>
        <p class="lg:text-lg text-sm text-justify">
          <strong>2019:</strong>
          Certificate for full stack web development program followed by exams
          and practical assignments in Code Academy - School of computer science
        </p>
        <p class="lg:text-lg text-sm text-justify">
          <strong>2019:</strong>
          &nbsp; Frontend Developer at N47
        </p>
        <p class="lg:text-lg text-sm text-justify">
          <strong>2021:</strong>
          &nbsp; Frontend Team Lead at Bransys
        </p>
      </div>
    </section>
  </div>
</template>

<script>
import TypeIt from 'typeit'
import 'animate.css'
import { ref } from 'vue'

export default {
  data() {
    return {
      display: false
    }
  },
  setup() {
    const landingIntro = ref('')

    if (landingIntro.value !== '') {
      landingIntro.value === ''
    }
  },
  mounted() {
    new TypeIt('#landing_intro', {
      speed: 50,
      waitUntilVisible: true,
      startDelay: 2000,
      startDelete: true,
      afterComplete: function (instance) {
        instance.destroy()
      }
    })
      .type('Hello! I am a full stack web developer based in Macedonia.')
      .go()
  }
}
</script>

<style>
.selector {
  background-color: var(--bg-sel);
}

.copyright {
  animation: fadeInLeft;
  animation-duration: 1s;
  animation-delay: 3s;
}
</style>
