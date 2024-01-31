<template>
  <section class="py-[200px] flex flex-col items-center justify-center px-4">
    <div class="text-[32px] font-semibold text-dark mb-4">Select Companies</div>
    <form class="w-full card">
      <div class="form-group">
        <label for="" class="text-grey">Companies</label>
        <p v-if="$fetchState.pending">Fetching mountains...</p>
        <select v-else v-model="selectedCompany" name="companies" id=""
          class="appearance-none input-field form-icon-chevron_down">
          <option :value="company.id" v-for="company in companies.data.data.data" :key="company.id">{{ company.name }}
          </option>
        </select>
      </div>
      <button @click="openCompany()" type="button" class="w-full btn btn-primary mt-[14px]">Continue</button>
      <div class="text-center">or</div>
      <NuxtLink :to="{ name: 'companies-create' }" class="w-full btn btn-white border mt-[14px]">
        Create New Company
      </NuxtLink>
    </form>
  </section>
</template>

<script>
export default {
  middleware: 'auth',
  data() {
    return {
      companies: [],
      selectedCompany: ''
    }
  },
  async fetch() {
    try {
      this.companies = await this.$axios.get('company');
    } catch (error) {
      console.error('There was an error fetching the companies:', error);
    }
  },
  methods: {
    openCompany() {
      this.$router.push({
        name: 'companies-id',
        params: {
          id: this.selectedCompany
        }
      })
    }
  }
}
</script>
