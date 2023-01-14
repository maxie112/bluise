<template>
  <section class="home">
    <div class="py-24 md:py-36 mx-auto flex flex-wrap flex-col md:flex-row items-center">
      <div class="flex flex-col w-full xl:w-3/5 justify-center lg:items-start overflow-y-hidden">
        <div v-html="$md.render(welcomeText)" class="home__welcome markdown" />

        <div class="mb-12 xl:mb-0">
          <h4 v-if="isSignedUp">Thank you - we'll be in touch shortly.</h4>

          <form
            v-else
            @submit.prevent="handleSubmit"
            name="signups"
            netlify
            class="flex items-center border-b border-b-2 border-blue-400 py-2"
          >
            <input
              ref="emailInput"
              v-model="form.email"
              class="appearance-none mb-36 bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
              type="text"
              name="email"
              placeholder="your@email.com"
              aria-label="Email address"
            />

            <button
              class="flex-shrink-0 bg-blue-500 hover:bg-blue-700 border-blue-500 hover:border-blue-700 text-sm border-4 text-white py-1 px-2 rounded"
              type="submit"
            >
              Sign Up
            </button>
          </form>
        </div>
      </div>
      <div class="flex flex-col w-full xl:w-2/5">
        <img
          alt="Hero"
          class="rounded shadow-xl"
          src="https://source.unsplash.com/random/720x400"
        />
      </div>
      <div class="flex flex-wrap md:-mx-4 pb-20">
  <div v-for="(post, index) in posts" :key="index" class="w-full md:w-1/2 my-4 md:px-4">
    <div class="post">
      <nuxt-link :to="`/blog/${post.slug}`">
        <img
          :alt="post.title"
          class="w-full"
          :src="post.featuredImage || 'https://source.unsplash.com/random/640x340'"
        />
        <div class="p-6 bg-white">
          <h2 class="text-2xl mb-2">{{ post.title }}</h2>

          <p class="text-base font-light">
            {{ post.excerpt }}
          </p>

          <h6 class="text-blue-600 mt-4 font-medium">Lees meer</h6>
        </div>
      </nuxt-link>
    </div>
  </div>
</div>

<div class="py-8 md:py-16 text-center">
  <h1 class="text-lg md:text-xl lg:text-4xl xl:text-6xl">Team</h1>
</div>
<div class="flex flex-wrap md:-mx-4 pb-20">
  <div v-for="(member, index) in team" :key="index" class="w-full md:w-1/3 my-4 md:px-4">
    <div class="team-member">
      <img :src="member.img" alt="team member" :alt="member.name" class="w-full">
      <h3 class="text-lg md:text-xl">{{ member.name }}</h3>
      <p class="text-base font-light">{{ member.position }}</p>
    </div>
  </div>
</div>
<Pagination v-if="totalPages > 1" :current-page="currentPage" :total-pages="totalPages" />

    </div>
  </section>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator';
import settings from '@/content/settings/general.json';

@Component({
  // Called to know which transition to apply
  transition() {
    return 'slide-left';
  },
})
export default class Home extends Vue {
  welcomeText = settings.welcomeText;

  get posts(): Post[] {
    return this.$store.state.posts;
  }

  isSignedUp = false;

  form = {
    email: '',
  };

  encode(data): string {
    return Object.keys(data)
      .map((key) => `${encodeURIComponent(key)}=${encodeURIComponent(data[key])}`)
      .join('&');
  }

  validEmail(email): boolean {
    // eslint-disable-next-line
    const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test(email);
  }

  async handleSubmit(): Promise<void> {
    if (!this.validEmail(this.form.email)) {
      this.$refs.emailInput.focus();
      return;
    }

    try {
      await fetch('/', {
        method: 'POST',
        headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
        body: this.encode({ 'form-name': 'signups', ...this.form }),
      });

      this.isSignedUp = true;
    } catch (error) {
      console.error(error);
    }
  }
}
</script>
