<template>
  <div>
    <h1 class="mb-8 font-bold text-3xl">Organizations</h1>
    <div class="mb-6 flex justify-between items-center">
      <search-filter v-model="form.search" class="w-full max-w-md mr-4" @reset="reset">
        <label class="block text-gray-700">Trashed:</label>
        <select v-model="form.trashed" class="mt-1 w-full form-select">
          <option :value="null" />
          <option value="with">With Trashed</option>
          <option value="only">Only Trashed</option>
        </select>
      </search-filter>
      <a class="btn-indigo" href="javaScript:void(0)" @click.prevent="createOrganizationDialog = true">
        <span>Create</span>
        <span class="hidden md:inline">Organization</span>
      </a>
    </div>
    <div class="bg-white rounded-md shadow overflow-x-auto">
      <table class="w-full whitespace-nowrap">
        <tr class="text-left font-bold">
          <th class="px-6 pt-6 pb-4">Name</th>
          <th class="px-6 pt-6 pb-4">City</th>
          <th class="px-6 pt-6 pb-4" colspan="2">Phone</th>
        </tr>
        <tr v-for="organization in organizations.data" :key="organization.id" class="hover:bg-gray-100 focus-within:bg-gray-100">
          <td class="border-t">
            <span class="px-6 py-4 flex items-center focus:text-indigo-500">
              {{ organization.name }}
              <icon v-if="organization.deleted_at" name="trash" class="flex-shrink-0 w-3 h-3 fill-gray-400 ml-2" />
            </span>
          </td>
          <td class="border-t">
            <span class="px-6 py-4 flex items-center" tabindex="-1">
              {{ organization.city }}
            </span>
          </td>
          <td class="border-t">
            <span class="px-6 py-4 flex items-center" tabindex="-1">
              {{ organization.phone }}
            </span>
          </td>
          <td class="border-t w-px">
            <a class="px-4 flex items-center" href="javaScript:void(0)" @click.prevent="edit(organization)" tabindex="-1">
              <icon name="cheveron-right" class="block w-6 h-6 fill-gray-400" />
            </a>
          </td>
        </tr>
        <tr v-if="organizations.data.length === 0">
          <td class="border-t px-6 py-4" colspan="4">No organizations found.</td>
        </tr>
      </table>
    </div>
    <pagination class="mt-6" :links="organizations.links" />
    <Dialog v-model="createOrganizationDialog" persistent>
      <Create
        @onClose="createOrganizationDialog = false"
      />
    </Dialog>

    <Dialog v-model="editOrganizationDialog" persistent>
      <Edit
        :organization="organization"
        @onClose="editOrganizationDialog = false"
      />
    </Dialog>
  </div>
</template>

<script>
import Icon from '@/Shared/Icon'
import pickBy from 'lodash/pickBy'
import Layout from '@/Shared/Layout'
import throttle from 'lodash/throttle'
import mapValues from 'lodash/mapValues'
import Pagination from '@/Shared/Pagination'
import SearchFilter from '@/Shared/SearchFilter'

import Dialog from './Dialog'
import Create from './Create'
import Edit from './Edit'
export default {
  metaInfo: { title: 'Organizations' },
  components: {
    Icon,
    Pagination,
    SearchFilter,
    Dialog,
    Create,
    Edit,
  },
  layout: Layout,
  props: {
    filters: Object,
    organizations: Object,
  },
  data() {
    return {
      createOrganizationDialog: false,
      editOrganizationDialog: false,
      organization: {},
      form: {
        search: this.filters.search,
        trashed: this.filters.trashed,
      },
    }
  },
  watch: {
    form: {
      deep: true,
      handler: throttle(function() {
        this.$inertia.get(this.route('organizations'), pickBy(this.form), { preserveState: true })
      }, 150),
    },
  },
  methods: {
    reset() {
      this.form = mapValues(this.form, () => null)
    },
    edit(organization) {
      console.log(organization.name, '====');
      this.organization = organization
      this.editOrganizationDialog =  true
    },
  },
}
</script>
