The following is a digest of the repository "sheba-admin-dashboard".
This digest is designed to be easily parsed by Large Language Models.

--- SUMMARY ---
Repository: sheba-admin-dashboard
Files Analyzed: 26
Total Text Size: 155.82 KB
Estimated Tokens (text only): ~38,450

--- DIRECTORY STRUCTURE ---
sheba-admin-dashboard/
├── src/
│   ├── components/
│   │   ├── ConfirmModal.vue
│   │   ├── MessageReplyModal.vue
│   │   ├── MessageViewModal.vue
│   │   ├── ProjectModal.vue
│   │   ├── ProjectViewModal.vue
│   │   ├── PublicationModal.vue
│   │   ├── PublicationViewModal.vue
│   │   ├── QuickActionButton.vue
│   │   ├── Sidebar.vue
│   │   ├── StatCard.vue
│   │   ├── Toast.vue
│   │   └── TopNavigation.vue
│   ├── data/
│   │   └── mockData.js
│   ├── router/
│   │   └── index.js
│   ├── views/
│   │   ├── Dashboard.vue
│   │   ├── Messages.vue
│   │   ├── Projects.vue
│   │   ├── Publications.vue
│   │   └── Settings.vue
│   ├── App.vue
│   ├── main.js
│   └── style.css
├── jsconfig.json
├── package.json
├── README.md
└── vite.config.js


--- FILE CONTENTS ---
============================================================
FILE: src/components/ConfirmModal.vue
============================================================
<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
    <div class="bg-white dark:bg-gray-800 rounded-xl max-w-md w-full">
      <!-- Header -->
      <div class="p-6 border-b border-gray-200 dark:border-gray-700">
        <div class="flex items-center space-x-3">
          <div class="w-10 h-10 bg-red-100 dark:bg-red-900 rounded-full flex items-center justify-center">
            <font-awesome-icon icon="exclamation-triangle" class="text-red-600 dark:text-red-400" />
          </div>
          <h3 class="text-lg font-semibold text-gray-900 dark:text-white">
            {{ title }}
          </h3>
        </div>
      </div>

      <!-- Content -->
      <div class="p-6">
        <p class="text-gray-600 dark:text-gray-400">
          {{ message }}
        </p>
      </div>

      <!-- Actions -->
      <div class="flex items-center justify-end space-x-4 p-6 border-t border-gray-200 dark:border-gray-700">
        <button
          @click="$emit('cancel')"
          class="px-4 py-2 text-gray-700 dark:text-gray-300 border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
        >
          Cancel
        </button>
        <button
          @click="$emit('confirm')"
          class="px-4 py-2 bg-red-600 text-white rounded-lg hover:bg-red-700 transition-colors"
        >
          Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ConfirmModal',
  props: {
    title: {
      type: String,
      required: true
    },
    message: {
      type: String,
      required: true
    }
  },
  emits: ['confirm', 'cancel']
}
</script>



============================================================
FILE: src/components/MessageReplyModal.vue
============================================================
<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
    <div class="bg-white dark:bg-gray-800 rounded-xl max-w-2xl w-full max-h-[90vh] overflow-y-auto">
      <!-- Header -->
      <div class="flex items-center justify-between p-6 border-b border-gray-200 dark:border-gray-700">
        <h3 class="text-lg font-semibold text-gray-900 dark:text-white">
          Reply to {{ message.name }}
        </h3>
        <button 
          @click="$emit('close')"
          class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300"
        >
          <font-awesome-icon icon="times" class="text-xl" />
        </button>
      </div>

      <!-- Original Message -->
      <div class="p-6 border-b border-gray-200 dark:border-gray-700 bg-gray-50 dark:bg-gray-700/50">
        <h4 class="text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Original Message:</h4>
        <div class="text-sm text-gray-600 dark:text-gray-400 space-y-1">
          <p><strong>From:</strong> {{ message.name }} ({{ message.email }})</p>
          <p><strong>Subject:</strong> {{ message.subject }}</p>
          <p><strong>Date:</strong> {{ formatDate(message.date) }}</p>
        </div>
        <div class="mt-3 p-3 bg-white dark:bg-gray-800 rounded-lg text-sm text-gray-700 dark:text-gray-300 max-h-32 overflow-y-auto">
          {{ message.message }}
        </div>
      </div>

      <!-- Reply Form -->
      <form @submit.prevent="handleSubmit" class="p-6 space-y-6">
        <!-- Reply Subject -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Subject
          </label>
          <input
            v-model="form.subject"
            type="text"
            readonly
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg bg-gray-50 dark:bg-gray-700 text-gray-600 dark:text-gray-400"
          />
        </div>

        <!-- Reply Message -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Your Reply *
          </label>
          <textarea
            v-model="form.message"
            required
            rows="8"
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-transparent dark:bg-gray-700 dark:text-white resize-none"
            placeholder="Type your reply here..."
          ></textarea>
          <p v-if="errors.message" class="text-red-500 text-sm mt-1">{{ errors.message }}</p>
        </div>

        <!-- Reply Options -->
        <div class="space-y-3">
          <div class="flex items-center">
            <input
              v-model="form.sendCopy"
              type="checkbox"
              id="sendCopy"
              class="w-4 h-4 text-green-600 bg-gray-100 border-gray-300 rounded focus:ring-green-500 dark:focus:ring-green-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600"
            />
            <label for="sendCopy" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
              Send a copy to my email
            </label>
          </div>
          
          <div class="flex items-center">
            <input
              v-model="form.markAsReplied"
              type="checkbox"
              id="markAsReplied"
              class="w-4 h-4 text-green-600 bg-gray-100 border-gray-300 rounded focus:ring-green-500 dark:focus:ring-green-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600"
            />
            <label for="markAsReplied" class="ml-2 text-sm text-gray-700 dark:text-gray-300">
              Mark message as replied
            </label>
          </div>
        </div>

        <!-- Quick Reply Templates -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Quick Templates
          </label>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-2">
            <button
              v-for="template in replyTemplates"
              :key="template.name"
              type="button"
              @click="useTemplate(template)"
              class="text-left p-3 text-sm border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
            >
              <div class="font-medium text-gray-900 dark:text-white">{{ template.name }}</div>
              <div class="text-gray-500 dark:text-gray-400 text-xs mt-1">{{ template.preview }}</div>
            </button>
          </div>
        </div>

        <!-- Form Actions -->
        <div class="flex items-center justify-end space-x-4 pt-6 border-t border-gray-200 dark:border-gray-700">
          <button
            type="button"
            @click="$emit('close')"
            class="px-4 py-2 text-gray-700 dark:text-gray-300 border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
          >
            Cancel
          </button>
          <button
            type="submit"
            :disabled="!form.message.trim()"
            class="px-6 py-2 bg-gradient-to-r from-green-500 to-teal-500 text-white rounded-lg hover:shadow-lg transition-all duration-300 disabled:opacity-50 disabled:cursor-not-allowed"
          >
            <font-awesome-icon icon="paper-plane" class="mr-2" />
            Send Reply
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MessageReplyModal',
  props: {
    message: {
      type: Object,
      required: true
    }
  },
  emits: ['close', 'send'],
  data() {
    return {
      form: {
        subject: '',
        message: '',
        sendCopy: false,
        markAsReplied: true
      },
      errors: {},
      replyTemplates: [
        {
          name: 'Thank You',
          preview: 'Thank you for your message...',
          content: `Dear ${this.message?.name || '[Name]'},

Thank you for reaching out to Sheba Youth Foundation. We have received your message and appreciate your interest in our work.

We will review your inquiry and get back to you as soon as possible.

Best regards,
Sheba Youth Foundation Team`
        },
        {
          name: 'Information Request',
          preview: 'Thank you for your interest...',
          content: `Dear ${this.message?.name || '[Name]'},

Thank you for your interest in Sheba Youth Foundation and our programs. We're pleased to provide you with the information you requested.

[Please add specific information here]

If you have any additional questions, please don't hesitate to contact us.

Best regards,
Sheba Youth Foundation Team`
        },
        {
          name: 'Meeting Request',
          preview: 'We would be happy to meet...',
          content: `Dear ${this.message?.name || '[Name]'},

Thank you for your message. We would be happy to arrange a meeting to discuss your inquiry further.

Please let us know your availability, and we will coordinate a suitable time for both parties.

Looking forward to hearing from you.

Best regards,
Sheba Youth Foundation Team`
        },
        {
          name: 'Follow Up',
          preview: 'Following up on your message...',
          content: `Dear ${this.message?.name || '[Name]'},

I hope this message finds you well. I'm following up on your recent inquiry to Sheba Youth Foundation.

[Please add specific follow-up information here]

Please feel free to reach out if you have any questions or need further assistance.

Best regards,
Sheba Youth Foundation Team`
        }
      ]
    }
  },
  mounted() {
    this.form.subject = `Re: ${this.message.subject}`
  },
  methods: {
    formatDate(dateString) {
      return new Date(dateString).toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      })
    },
    
    useTemplate(template) {
      this.form.message = template.content
    },
    
    validateForm() {
      this.errors = {}
      
      if (!this.form.message.trim()) {
        this.errors.message = 'Reply message is required'
      }
      
      return Object.keys(this.errors).length === 0
    },
    
    handleSubmit() {
      if (this.validateForm()) {
        this.$emit('send', {
          ...this.form,
          originalMessageId: this.message.id,
          recipientEmail: this.message.email,
          recipientName: this.message.name
        })
      }
    }
  }
}
</script>



============================================================
FILE: src/components/MessageViewModal.vue
============================================================
<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
    <div class="bg-white dark:bg-gray-800 rounded-xl max-w-3xl w-full max-h-[90vh] overflow-y-auto">
      <!-- Header -->
      <div class="flex items-center justify-between p-6 border-b border-gray-200 dark:border-gray-700">
        <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
          Message Details
        </h3>
        <button 
          @click="$emit('close')"
          class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300"
        >
          <font-awesome-icon icon="times" class="text-xl" />
        </button>
      </div>

      <!-- Content -->
      <div class="p-6">
        <!-- Sender Information -->
        <div class="flex items-start space-x-4 mb-6">
          <div class="w-16 h-16 bg-gradient-to-r from-blue-500 to-purple-500 rounded-full flex items-center justify-center text-white text-xl font-bold">
            {{ message.name.charAt(0).toUpperCase() }}
          </div>
          <div class="flex-1">
            <div class="flex items-center space-x-3 mb-2">
              <h2 class="text-xl font-bold text-gray-900 dark:text-white">{{ message.name }}</h2>
              <span v-if="!message.isRead" class="px-2 py-1 bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-300 text-xs font-medium rounded-full">
                Unread
              </span>
              <span v-if="message.isReplied" class="px-2 py-1 bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300 text-xs font-medium rounded-full">
                Replied
              </span>
            </div>
            
            <div class="space-y-1 text-sm text-gray-600 dark:text-gray-400">
              <div class="flex items-center space-x-2">
                <font-awesome-icon icon="envelope" />
                <a :href="`mailto:${message.email}`" class="hover:text-blue-600 dark:hover:text-blue-400">
                  {{ message.email }}
                </a>
              </div>
              <div v-if="message.phone" class="flex items-center space-x-2">
                <font-awesome-icon icon="phone" />
                <a :href="`tel:${message.phone}`" class="hover:text-blue-600 dark:hover:text-blue-400">
                  {{ message.phone }}
                </a>
              </div>
              <div class="flex items-center space-x-2">
                <font-awesome-icon icon="clock" />
                <span>{{ formatDate(message.date) }}</span>
              </div>
            </div>
          </div>
        </div>

        <!-- Message Subject -->
        <div class="mb-6">
          <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-2">Subject</h3>
          <p class="text-gray-700 dark:text-gray-300 bg-gray-50 dark:bg-gray-700 p-4 rounded-lg">
            {{ message.subject }}
          </p>
        </div>

        <!-- Message Content -->
        <div class="mb-6">
          <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-2">Message</h3>
          <div class="text-gray-700 dark:text-gray-300 bg-gray-50 dark:bg-gray-700 p-4 rounded-lg whitespace-pre-wrap">
            {{ message.message }}
          </div>
        </div>

        <!-- Reply Section (if replied) -->
        <div v-if="message.isReplied && message.replyMessage" class="mb-6">
          <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-2">Your Reply</h3>
          <div class="bg-blue-50 dark:bg-blue-900/20 border-l-4 border-blue-500 p-4 rounded-r-lg">
            <div class="flex items-center space-x-2 mb-2 text-sm text-gray-600 dark:text-gray-400">
              <font-awesome-icon icon="reply" />
              <span>Replied on {{ formatDate(message.replyDate) }}</span>
            </div>
            <p class="text-gray-700 dark:text-gray-300 whitespace-pre-wrap">{{ message.replyMessage }}</p>
          </div>
        </div>
      </div>

      <!-- Actions -->
      <div class="flex items-center justify-between p-6 border-t border-gray-200 dark:border-gray-700">
        <div class="flex items-center space-x-3">
          <button
            @click="$emit('toggleRead', message)"
            :class="[
              'px-4 py-2 rounded-lg transition-colors border',
              message.isRead 
                ? 'text-gray-600 dark:text-gray-400 border-gray-300 dark:border-gray-600 hover:bg-gray-50 dark:hover:bg-gray-700' 
                : 'text-blue-600 dark:text-blue-400 border-blue-300 dark:border-blue-600 hover:bg-blue-50 dark:hover:bg-blue-900/20'
            ]"
          >
            <font-awesome-icon :icon="message.isRead ? 'envelope-open' : 'envelope'" class="mr-2" />
            {{ message.isRead ? 'Mark Unread' : 'Mark Read' }}
          </button>
          
          <button
            @click="$emit('reply', message)"
            class="px-4 py-2 text-green-600 dark:text-green-400 border border-green-300 dark:border-green-600 rounded-lg hover:bg-green-50 dark:hover:bg-green-900/20 transition-colors"
          >
            <font-awesome-icon icon="reply" class="mr-2" />
            Reply
          </button>
          
          <button
            @click="$emit('delete', message)"
            class="px-4 py-2 text-red-600 dark:text-red-400 border border-red-300 dark:border-red-600 rounded-lg hover:bg-red-50 dark:hover:bg-red-900/20 transition-colors"
          >
            <font-awesome-icon icon="trash" class="mr-2" />
            Delete
          </button>
        </div>
        
        <button
          @click="$emit('close')"
          class="px-6 py-2 text-gray-700 dark:text-gray-300 border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
        >
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MessageViewModal',
  props: {
    message: {
      type: Object,
      required: true
    }
  },
  emits: ['close', 'reply', 'delete', 'toggleRead'],
  methods: {
    formatDate(dateString) {
      return new Date(dateString).toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      })
    }
  }
}
</script>



============================================================
FILE: src/components/ProjectModal.vue
============================================================
<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
    <div class="bg-white dark:bg-gray-800 rounded-xl max-w-2xl w-full max-h-[90vh] overflow-y-auto">
      <!-- Header -->
      <div class="flex items-center justify-between p-6 border-b border-gray-200 dark:border-gray-700">
        <h3 class="text-lg font-semibold text-gray-900 dark:text-white">
          {{ isEdit ? 'Edit Project' : 'Add New Project' }}
        </h3>
        <button 
          @click="$emit('close')"
          class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300"
        >
          <font-awesome-icon icon="times" class="text-xl" />
        </button>
      </div>

      <!-- Form -->
      <form @submit.prevent="handleSubmit" class="p-6 space-y-6">
        <!-- Title -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Project Title *
          </label>
          <input
            v-model="form.title"
            type="text"
            required
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            placeholder="Enter project title"
          />
          <p v-if="errors.title" class="text-red-500 text-sm mt-1">{{ errors.title }}</p>
        </div>

        <!-- Description -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Description *
          </label>
          <textarea
            v-model="form.description"
            required
            rows="4"
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            placeholder="Enter project description"
          ></textarea>
          <p v-if="errors.description" class="text-red-500 text-sm mt-1">{{ errors.description }}</p>
        </div>

        <!-- Image URL -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Image URL
          </label>
          <input
            v-model="form.image"
            type="url"
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            placeholder="https://example.com/image.jpg"
          />
          <p class="text-gray-500 dark:text-gray-400 text-sm mt-1">
            Leave empty to use default image
          </p>
        </div>

        <!-- Location and Beneficiaries -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
              Location *
            </label>
            <input
              v-model="form.location"
              type="text"
              required
              class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
              placeholder="e.g., Taiz Governorate"
            />
            <p v-if="errors.location" class="text-red-500 text-sm mt-1">{{ errors.location }}</p>
          </div>
          
          <div>
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
              Beneficiaries *
            </label>
            <input
              v-model="form.beneficiaries"
              type="text"
              required
              class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
              placeholder="e.g., 100+ Beneficiaries"
            />
            <p v-if="errors.beneficiaries" class="text-red-500 text-sm mt-1">{{ errors.beneficiaries }}</p>
          </div>
        </div>

        <!-- Status and Category -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
              Status *
            </label>
            <select
              v-model="form.status"
              required
              class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            >
              <option value="">Select Status</option>
              <option value="Active">Active</option>
              <option value="Completed">Completed</option>
              <option value="Planning">Planning</option>
              <option value="On Hold">On Hold</option>
            </select>
            <p v-if="errors.status" class="text-red-500 text-sm mt-1">{{ errors.status }}</p>
          </div>
          
          <div>
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
              Category *
            </label>
            <select
              v-model="form.category"
              required
              class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            >
              <option value="">Select Category</option>
              <option value="Community Development">Community Development</option>
              <option value="Youth Development">Youth Development</option>
              <option value="Capacity Building">Capacity Building</option>
              <option value="Media Development">Media Development</option>
            </select>
            <p v-if="errors.category" class="text-red-500 text-sm mt-1">{{ errors.category }}</p>
          </div>
        </div>

        <!-- Dates and Budget -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
              Start Date *
            </label>
            <input
              v-model="form.startDate"
              type="date"
              required
              class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            />
            <p v-if="errors.startDate" class="text-red-500 text-sm mt-1">{{ errors.startDate }}</p>
          </div>
          
          <div>
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
              End Date
            </label>
            <input
              v-model="form.endDate"
              type="date"
              class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            />
          </div>
          
          <div>
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
              Budget (USD)
            </label>
            <input
              v-model.number="form.budget"
              type="number"
              min="0"
              class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
              placeholder="0"
            />
          </div>
        </div>

        <!-- Form Actions -->
        <div class="flex items-center justify-end space-x-4 pt-6 border-t border-gray-200 dark:border-gray-700">
          <button
            type="button"
            @click="$emit('close')"
            class="px-4 py-2 text-gray-700 dark:text-gray-300 border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
          >
            Cancel
          </button>
          <button
            type="submit"
            :disabled="!isFormValid"
            class="px-6 py-2 bg-gradient-to-r from-cyan-500 to-orange-500 text-white rounded-lg hover:shadow-lg transition-all duration-300 disabled:opacity-50 disabled:cursor-not-allowed"
          >
            {{ isEdit ? 'Update Project' : 'Create Project' }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProjectModal',
  props: {
    project: {
      type: Object,
      default: null
    },
    isEdit: {
      type: Boolean,
      default: false
    }
  },
  emits: ['close', 'save'],
  data() {
    return {
      form: {
        title: '',
        description: '',
        image: '',
        location: '',
        beneficiaries: '',
        status: '',
        category: '',
        startDate: '',
        endDate: '',
        budget: null
      },
      errors: {}
    }
  },
  computed: {
    isFormValid() {
      return this.form.title && 
             this.form.description && 
             this.form.location && 
             this.form.beneficiaries && 
             this.form.status && 
             this.form.category && 
             this.form.startDate
    }
  },
  mounted() {
    if (this.isEdit && this.project) {
      this.form = { ...this.project }
    } else {
      // Set default image for new projects
      this.form.image = '/photos/sheba-logo.png'
    }
  },
  methods: {
    validateForm() {
      this.errors = {}
      
      if (!this.form.title) {
        this.errors.title = 'Project title is required'
      }
      
      if (!this.form.description) {
        this.errors.description = 'Project description is required'
      }
      
      if (!this.form.location) {
        this.errors.location = 'Location is required'
      }
      
      if (!this.form.beneficiaries) {
        this.errors.beneficiaries = 'Beneficiaries information is required'
      }
      
      if (!this.form.status) {
        this.errors.status = 'Status is required'
      }
      
      if (!this.form.category) {
        this.errors.category = 'Category is required'
      }
      
      if (!this.form.startDate) {
        this.errors.startDate = 'Start date is required'
      }
      
      if (this.form.endDate && this.form.startDate && new Date(this.form.endDate) < new Date(this.form.startDate)) {
        this.errors.endDate = 'End date must be after start date'
      }
      
      return Object.keys(this.errors).length === 0
    },
    
    handleSubmit() {
      if (this.validateForm()) {
        // Set default image if not provided
        if (!this.form.image) {
          this.form.image = '/photos/sheba-logo.png'
        }
        
        this.$emit('save', { ...this.form })
      }
    }
  }
}
</script>



============================================================
FILE: src/components/ProjectViewModal.vue
============================================================
<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
    <div class="bg-white dark:bg-gray-800 rounded-xl max-w-4xl w-full max-h-[90vh] overflow-y-auto">
      <!-- Header -->
      <div class="flex items-center justify-between p-6 border-b border-gray-200 dark:border-gray-700">
        <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
          Project Details
        </h3>
        <button 
          @click="$emit('close')"
          class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300"
        >
          <font-awesome-icon icon="times" class="text-xl" />
        </button>
      </div>

      <!-- Content -->
      <div class="p-6">
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
          <!-- Project Image -->
          <div class="lg:col-span-1">
            <img 
              :src="project.image" 
              :alt="project.title"
              class="w-full h-64 object-cover rounded-lg"
            />
          </div>

          <!-- Project Information -->
          <div class="lg:col-span-2 space-y-6">
            <!-- Title and Status -->
            <div>
              <h2 class="text-2xl font-bold text-gray-900 dark:text-white mb-2">
                {{ project.title }}
              </h2>
              <span :class="getStatusClass(project.status)" class="px-3 py-1 text-sm font-medium rounded-full">
                {{ project.status }}
              </span>
            </div>

            <!-- Description -->
            <div>
              <h4 class="text-lg font-semibold text-gray-900 dark:text-white mb-2">Description</h4>
              <p class="text-gray-600 dark:text-gray-400 leading-relaxed">
                {{ project.description }}
              </p>
            </div>

            <!-- Project Details Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <!-- Location -->
              <div class="flex items-start space-x-3">
                <div class="w-10 h-10 bg-cyan-100 dark:bg-cyan-900 rounded-lg flex items-center justify-center flex-shrink-0">
                  <font-awesome-icon icon="map-marker-alt" class="text-cyan-600 dark:text-cyan-400" />
                </div>
                <div>
                  <h5 class="font-medium text-gray-900 dark:text-white">Location</h5>
                  <p class="text-gray-600 dark:text-gray-400">{{ project.location }}</p>
                </div>
              </div>

              <!-- Beneficiaries -->
              <div class="flex items-start space-x-3">
                <div class="w-10 h-10 bg-orange-100 dark:bg-orange-900 rounded-lg flex items-center justify-center flex-shrink-0">
                  <font-awesome-icon icon="users" class="text-orange-600 dark:text-orange-400" />
                </div>
                <div>
                  <h5 class="font-medium text-gray-900 dark:text-white">Beneficiaries</h5>
                  <p class="text-gray-600 dark:text-gray-400">{{ project.beneficiaries }}</p>
                </div>
              </div>

              <!-- Category -->
              <div class="flex items-start space-x-3">
                <div class="w-10 h-10 bg-purple-100 dark:bg-purple-900 rounded-lg flex items-center justify-center flex-shrink-0">
                  <font-awesome-icon icon="tag" class="text-purple-600 dark:text-purple-400" />
                </div>
                <div>
                  <h5 class="font-medium text-gray-900 dark:text-white">Category</h5>
                  <p class="text-gray-600 dark:text-gray-400">{{ project.category }}</p>
                </div>
              </div>

              <!-- Budget -->
              <div class="flex items-start space-x-3" v-if="project.budget">
                <div class="w-10 h-10 bg-green-100 dark:bg-green-900 rounded-lg flex items-center justify-center flex-shrink-0">
                  <span class="text-green-600 dark:text-green-400 font-bold">$</span>
                </div>
                <div>
                  <h5 class="font-medium text-gray-900 dark:text-white">Budget</h5>
                  <p class="text-gray-600 dark:text-gray-400">${{ project.budget.toLocaleString() }} USD</p>
                </div>
              </div>
            </div>

            <!-- Timeline -->
            <div v-if="project.startDate">
              <h4 class="text-lg font-semibold text-gray-900 dark:text-white mb-4">Timeline</h4>
              <div class="flex items-center space-x-4">
                <div class="flex items-center space-x-2">
                  <font-awesome-icon icon="calendar" class="text-gray-400" />
                  <span class="text-sm text-gray-600 dark:text-gray-400">Start:</span>
                  <span class="text-sm font-medium text-gray-900 dark:text-white">
                    {{ formatDate(project.startDate) }}
                  </span>
                </div>
                <div v-if="project.endDate" class="flex items-center space-x-2">
                  <font-awesome-icon icon="calendar" class="text-gray-400" />
                  <span class="text-sm text-gray-600 dark:text-gray-400">End:</span>
                  <span class="text-sm font-medium text-gray-900 dark:text-white">
                    {{ formatDate(project.endDate) }}
                  </span>
                </div>
              </div>
              
              <!-- Progress Bar (if project has end date) -->
              <div v-if="project.endDate" class="mt-4">
                <div class="flex items-center justify-between mb-2">
                  <span class="text-sm text-gray-600 dark:text-gray-400">Progress</span>
                  <span class="text-sm font-medium text-gray-900 dark:text-white">{{ projectProgress }}%</span>
                </div>
                <div class="w-full bg-gray-200 dark:bg-gray-700 rounded-full h-2">
                  <div 
                    class="bg-gradient-to-r from-cyan-500 to-orange-500 h-2 rounded-full transition-all duration-300"
                    :style="{ width: `${projectProgress}%` }"
                  ></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Footer -->
      <div class="flex items-center justify-end space-x-4 p-6 border-t border-gray-200 dark:border-gray-700">
        <button
          @click="$emit('close')"
          class="px-6 py-2 text-gray-700 dark:text-gray-300 border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
        >
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProjectViewModal',
  props: {
    project: {
      type: Object,
      required: true
    }
  },
  emits: ['close'],
  computed: {
    projectProgress() {
      if (!this.project.startDate || !this.project.endDate) {
        return 0
      }
      
      const start = new Date(this.project.startDate)
      const end = new Date(this.project.endDate)
      const now = new Date()
      
      if (now < start) return 0
      if (now > end) return 100
      
      const total = end - start
      const elapsed = now - start
      
      return Math.round((elapsed / total) * 100)
    }
  },
  methods: {
    getStatusClass(status) {
      const classes = {
        'Active': 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300',
        'Completed': 'bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-300',
        'Planning': 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900 dark:text-yellow-300',
        'On Hold': 'bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-300'
      }
      return classes[status] || 'bg-gray-100 text-gray-800 dark:bg-gray-900 dark:text-gray-300'
    },
    
    formatDate(dateString) {
      return new Date(dateString).toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'long',
        day: 'numeric'
      })
    }
  }
}
</script>



============================================================
FILE: src/components/PublicationModal.vue
============================================================
<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
    <div class="bg-white dark:bg-gray-800 rounded-xl max-w-3xl w-full max-h-[90vh] overflow-y-auto">
      <!-- Header -->
      <div class="flex items-center justify-between p-6 border-b border-gray-200 dark:border-gray-700">
        <h3 class="text-lg font-semibold text-gray-900 dark:text-white">
          {{ isEdit ? 'Edit Publication' : 'Add New Publication' }}
        </h3>
        <button 
          @click="$emit('close')"
          class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300"
        >
          <font-awesome-icon icon="times" class="text-xl" />
        </button>
      </div>

      <!-- Form -->
      <form @submit.prevent="handleSubmit" class="p-6 space-y-6">
        <!-- Title -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Publication Title *
          </label>
          <input
            v-model="form.title"
            type="text"
            required
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            placeholder="Enter publication title"
          />
          <p v-if="errors.title" class="text-red-500 text-sm mt-1">{{ errors.title }}</p>
        </div>

        <!-- Subtitle -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Subtitle
          </label>
          <input
            v-model="form.subtitle"
            type="text"
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            placeholder="Enter subtitle (optional)"
          />
        </div>

        <!-- Description -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Description *
          </label>
          <textarea
            v-model="form.description"
            required
            rows="4"
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            placeholder="Enter publication description"
          ></textarea>
          <p v-if="errors.description" class="text-red-500 text-sm mt-1">{{ errors.description }}</p>
        </div>

        <!-- Cover Image URL -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Cover Image URL
          </label>
          <input
            v-model="form.cover"
            type="url"
            class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            placeholder="https://example.com/cover.jpg"
          />
          <p class="text-gray-500 dark:text-gray-400 text-sm mt-1">
            Leave empty to use default cover
          </p>
        </div>

        <!-- Type and Year -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
              Publication Type *
            </label>
            <select
              v-model="form.type"
              required
              class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
            >
              <option value="">Select Type</option>
              <option value="Policy Brief">Policy Brief</option>
              <option value="Manual">Manual</option>
              <option value="Report">Report</option>
              <option value="Guide">Guide</option>
            </select>
            <p v-if="errors.type" class="text-red-500 text-sm mt-1">{{ errors.type }}</p>
          </div>
          
          <div>
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
              Publication Year *
            </label>
            <input
              v-model.number="form.year"
              type="number"
              required
              min="2000"
              :max="new Date().getFullYear()"
              class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
              placeholder="2024"
            />
            <p v-if="errors.year" class="text-red-500 text-sm mt-1">{{ errors.year }}</p>
          </div>
        </div>

        <!-- Authors -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Authors
          </label>
          <div class="space-y-2">
            <div 
              v-for="(author, index) in form.authors" 
              :key="index"
              class="flex items-center space-x-2"
            >
              <input
                v-model="form.authors[index]"
                type="text"
                class="flex-1 px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
                placeholder="Author name"
              />
              <button
                type="button"
                @click="removeAuthor(index)"
                class="text-red-600 hover:text-red-800 dark:text-red-400 dark:hover:text-red-300"
              >
                <font-awesome-icon icon="times" />
              </button>
            </div>
            <button
              type="button"
              @click="addAuthor"
              class="text-purple-600 hover:text-purple-800 dark:text-purple-400 dark:hover:text-purple-300 text-sm"
            >
              <font-awesome-icon icon="plus" class="mr-1" />
              Add Author
            </button>
          </div>
        </div>

        <!-- Pages and PDF URL -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
              Number of Pages
            </label>
            <input
              v-model.number="form.pages"
              type="number"
              min="1"
              class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
              placeholder="0"
            />
          </div>
          
          <div>
            <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
              PDF Download URL
            </label>
            <input
              v-model="form.pdfUrl"
              type="url"
              class="w-full px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
              placeholder="https://example.com/document.pdf"
            />
          </div>
        </div>

        <!-- Tags -->
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">
            Tags
          </label>
          <div class="space-y-2">
            <div class="flex flex-wrap gap-2">
              <span 
                v-for="(tag, index) in form.tags" 
                :key="index"
                class="inline-flex items-center px-3 py-1 bg-purple-100 dark:bg-purple-900 text-purple-800 dark:text-purple-300 text-sm rounded-full"
              >
                {{ tag }}
                <button
                  type="button"
                  @click="removeTag(index)"
                  class="ml-2 text-purple-600 hover:text-purple-800 dark:text-purple-400 dark:hover:text-purple-300"
                >
                  <font-awesome-icon icon="times" class="text-xs" />
                </button>
              </span>
            </div>
            <div class="flex items-center space-x-2">
              <input
                v-model="newTag"
                type="text"
                @keyup.enter="addTag"
                class="flex-1 px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
                placeholder="Add a tag and press Enter"
              />
              <button
                type="button"
                @click="addTag"
                class="px-4 py-2 text-purple-600 hover:text-purple-800 dark:text-purple-400 dark:hover:text-purple-300 border border-purple-300 dark:border-purple-600 rounded-lg"
              >
                Add
              </button>
            </div>
          </div>
        </div>

        <!-- Form Actions -->
        <div class="flex items-center justify-end space-x-4 pt-6 border-t border-gray-200 dark:border-gray-700">
          <button
            type="button"
            @click="$emit('close')"
            class="px-4 py-2 text-gray-700 dark:text-gray-300 border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
          >
            Cancel
          </button>
          <button
            type="submit"
            :disabled="!isFormValid"
            class="px-6 py-2 bg-gradient-to-r from-purple-500 to-pink-500 text-white rounded-lg hover:shadow-lg transition-all duration-300 disabled:opacity-50 disabled:cursor-not-allowed"
          >
            {{ isEdit ? 'Update Publication' : 'Create Publication' }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PublicationModal',
  props: {
    publication: {
      type: Object,
      default: null
    },
    isEdit: {
      type: Boolean,
      default: false
    }
  },
  emits: ['close', 'save'],
  data() {
    return {
      form: {
        title: '',
        subtitle: '',
        description: '',
        cover: '',
        type: '',
        year: new Date().getFullYear(),
        authors: [''],
        pages: null,
        pdfUrl: '',
        tags: []
      },
      newTag: '',
      errors: {}
    }
  },
  computed: {
    isFormValid() {
      return this.form.title && 
             this.form.description && 
             this.form.type && 
             this.form.year
    }
  },
  mounted() {
    if (this.isEdit && this.publication) {
      this.form = { 
        ...this.publication,
        authors: this.publication.authors ? [...this.publication.authors] : [''],
        tags: this.publication.tags ? [...this.publication.tags] : []
      }
    } else {
      // Set default cover for new publications
      this.form.cover = '/photos/sheba-logo.png'
    }
  },
  methods: {
    addAuthor() {
      this.form.authors.push('')
    },
    
    removeAuthor(index) {
      if (this.form.authors.length > 1) {
        this.form.authors.splice(index, 1)
      }
    },
    
    addTag() {
      if (this.newTag.trim() && !this.form.tags.includes(this.newTag.trim())) {
        this.form.tags.push(this.newTag.trim())
        this.newTag = ''
      }
    },
    
    removeTag(index) {
      this.form.tags.splice(index, 1)
    },
    
    validateForm() {
      this.errors = {}
      
      if (!this.form.title) {
        this.errors.title = 'Publication title is required'
      }
      
      if (!this.form.description) {
        this.errors.description = 'Publication description is required'
      }
      
      if (!this.form.type) {
        this.errors.type = 'Publication type is required'
      }
      
      if (!this.form.year || this.form.year < 2000 || this.form.year > new Date().getFullYear()) {
        this.errors.year = 'Valid publication year is required'
      }
      
      return Object.keys(this.errors).length === 0
    },
    
    handleSubmit() {
      if (this.validateForm()) {
        // Set default cover if not provided
        if (!this.form.cover) {
          this.form.cover = '/photos/sheba-logo.png'
        }
        
        // Filter out empty authors
        this.form.authors = this.form.authors.filter(author => author.trim())
        
        this.$emit('save', { ...this.form })
      }
    }
  }
}
</script>



============================================================
FILE: src/components/PublicationViewModal.vue
============================================================
<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
    <div class="bg-white dark:bg-gray-800 rounded-xl max-w-4xl w-full max-h-[90vh] overflow-y-auto">
      <!-- Header -->
      <div class="flex items-center justify-between p-6 border-b border-gray-200 dark:border-gray-700">
        <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
          Publication Details
        </h3>
        <button 
          @click="$emit('close')"
          class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300"
        >
          <font-awesome-icon icon="times" class="text-xl" />
        </button>
      </div>

      <!-- Content -->
      <div class="p-6">
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
          <!-- Cover Image -->
          <div class="lg:col-span-1">
            <div class="relative">
              <img 
                :src="publication.cover" 
                :alt="publication.title"
                class="w-full h-80 object-cover rounded-lg shadow-lg"
              />
              <div class="absolute top-3 right-3">
                <span :class="getTypeClass(publication.type)" class="px-3 py-1 text-sm font-medium rounded-full">
                  {{ publication.type }}
                </span>
              </div>
            </div>
            
            <!-- Download Button -->
            <button 
              v-if="publication.pdfUrl"
              @click="downloadPDF"
              class="w-full mt-4 px-4 py-3 bg-gradient-to-r from-purple-500 to-pink-500 text-white rounded-lg hover:shadow-lg transition-all duration-300 flex items-center justify-center"
            >
              <font-awesome-icon icon="download" class="mr-2" />
              Download PDF
            </button>
          </div>

          <!-- Publication Information -->
          <div class="lg:col-span-2 space-y-6">
            <!-- Title and Subtitle -->
            <div>
              <h2 class="text-2xl font-bold text-gray-900 dark:text-white mb-2">
                {{ publication.title }}
              </h2>
              <p v-if="publication.subtitle" class="text-lg text-gray-600 dark:text-gray-400 mb-4">
                {{ publication.subtitle }}
              </p>
            </div>

            <!-- Description -->
            <div>
              <h4 class="text-lg font-semibold text-gray-900 dark:text-white mb-2">Description</h4>
              <p class="text-gray-600 dark:text-gray-400 leading-relaxed">
                {{ publication.description }}
              </p>
            </div>

            <!-- Publication Details Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <!-- Authors -->
              <div v-if="publication.authors && publication.authors.length" class="flex items-start space-x-3">
                <div class="w-10 h-10 bg-purple-100 dark:bg-purple-900 rounded-lg flex items-center justify-center flex-shrink-0">
                  <font-awesome-icon icon="user" class="text-purple-600 dark:text-purple-400" />
                </div>
                <div>
                  <h5 class="font-medium text-gray-900 dark:text-white">Authors</h5>
                  <div class="text-gray-600 dark:text-gray-400">
                    <p v-for="author in publication.authors" :key="author" class="text-sm">
                      {{ author }}
                    </p>
                  </div>
                </div>
              </div>

              <!-- Publication Year -->
              <div class="flex items-start space-x-3">
                <div class="w-10 h-10 bg-blue-100 dark:bg-blue-900 rounded-lg flex items-center justify-center flex-shrink-0">
                  <font-awesome-icon icon="calendar" class="text-blue-600 dark:text-blue-400" />
                </div>
                <div>
                  <h5 class="font-medium text-gray-900 dark:text-white">Publication Year</h5>
                  <p class="text-gray-600 dark:text-gray-400">{{ publication.year }}</p>
                </div>
              </div>

              <!-- Pages -->
              <div v-if="publication.pages" class="flex items-start space-x-3">
                <div class="w-10 h-10 bg-green-100 dark:bg-green-900 rounded-lg flex items-center justify-center flex-shrink-0">
                  <font-awesome-icon icon="file-alt" class="text-green-600 dark:text-green-400" />
                </div>
                <div>
                  <h5 class="font-medium text-gray-900 dark:text-white">Pages</h5>
                  <p class="text-gray-600 dark:text-gray-400">{{ publication.pages }} pages</p>
                </div>
              </div>

              <!-- Downloads -->
              <div v-if="publication.downloadCount" class="flex items-start space-x-3">
                <div class="w-10 h-10 bg-orange-100 dark:bg-orange-900 rounded-lg flex items-center justify-center flex-shrink-0">
                  <font-awesome-icon icon="download" class="text-orange-600 dark:text-orange-400" />
                </div>
                <div>
                  <h5 class="font-medium text-gray-900 dark:text-white">Downloads</h5>
                  <p class="text-gray-600 dark:text-gray-400">{{ publication.downloadCount.toLocaleString() }} downloads</p>
                </div>
              </div>
            </div>

            <!-- Tags -->
            <div v-if="publication.tags && publication.tags.length">
              <h4 class="text-lg font-semibold text-gray-900 dark:text-white mb-3">Tags</h4>
              <div class="flex flex-wrap gap-2">
                <span 
                  v-for="tag in publication.tags" 
                  :key="tag"
                  class="px-3 py-1 bg-purple-100 dark:bg-purple-900 text-purple-800 dark:text-purple-300 text-sm rounded-full"
                >
                  {{ tag }}
                </span>
              </div>
            </div>

            <!-- Publication Date -->
            <div v-if="publication.publishDate" class="text-sm text-gray-500 dark:text-gray-400">
              <font-awesome-icon icon="clock" class="mr-2" />
              Published on {{ formatDate(publication.publishDate) }}
            </div>
          </div>
        </div>
      </div>

      <!-- Footer -->
      <div class="flex items-center justify-end space-x-4 p-6 border-t border-gray-200 dark:border-gray-700">
        <button
          @click="$emit('close')"
          class="px-6 py-2 text-gray-700 dark:text-gray-300 border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
        >
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PublicationViewModal',
  props: {
    publication: {
      type: Object,
      required: true
    }
  },
  emits: ['close'],
  methods: {
    getTypeClass(type) {
      const classes = {
        'Policy Brief': 'bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-300',
        'Manual': 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300',
        'Report': 'bg-purple-100 text-purple-800 dark:bg-purple-900 dark:text-purple-300',
        'Guide': 'bg-orange-100 text-orange-800 dark:bg-orange-900 dark:text-orange-300'
      }
      return classes[type] || 'bg-gray-100 text-gray-800 dark:bg-gray-900 dark:text-gray-300'
    },
    
    formatDate(dateString) {
      return new Date(dateString).toLocaleDateString('en-US', {
        year: 'numeric',
        month: 'long',
        day: 'numeric'
      })
    },
    
    downloadPDF() {
      // Simulate PDF download
      window.open(this.publication.pdfUrl, '_blank')
    }
  }
}
</script>



============================================================
FILE: src/components/QuickActionButton.vue
============================================================
<template>
  <button 
    @click="$emit('click')"
    class="p-4 bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-lg hover:shadow-md transition-all duration-300 text-left group hover:border-cyan-300 dark:hover:border-cyan-600"
  >
    <div class="flex items-center space-x-3">
      <div :class="iconBgColor" class="w-10 h-10 rounded-lg flex items-center justify-center group-hover:scale-110 transition-transform duration-300">
        <font-awesome-icon :icon="icon" :class="iconColor" />
      </div>
      <div class="flex-1">
        <h4 class="font-medium text-gray-900 dark:text-white group-hover:text-cyan-600 dark:group-hover:text-cyan-400 transition-colors">
          {{ title }}
        </h4>
        <p class="text-sm text-gray-500 dark:text-gray-400">{{ description }}</p>
      </div>
    </div>
  </button>
</template>

<script>
export default {
  name: 'QuickActionButton',
  props: {
    title: {
      type: String,
      required: true
    },
    description: {
      type: String,
      required: true
    },
    icon: {
      type: String,
      required: true
    },
    color: {
      type: String,
      default: 'blue'
    }
  },
  emits: ['click'],
  computed: {
    iconBgColor() {
      const colors = {
        cyan: 'bg-cyan-100 dark:bg-cyan-900',
        purple: 'bg-purple-100 dark:bg-purple-900',
        orange: 'bg-orange-100 dark:bg-orange-900',
        green: 'bg-green-100 dark:bg-green-900',
        blue: 'bg-blue-100 dark:bg-blue-900',
        red: 'bg-red-100 dark:bg-red-900',
        gray: 'bg-gray-100 dark:bg-gray-700'
      }
      return colors[this.color] || colors.blue
    },
    iconColor() {
      const colors = {
        cyan: 'text-cyan-600 dark:text-cyan-400',
        purple: 'text-purple-600 dark:text-purple-400',
        orange: 'text-orange-600 dark:text-orange-400',
        green: 'text-green-600 dark:text-green-400',
        blue: 'text-blue-600 dark:text-blue-400',
        red: 'text-red-600 dark:text-red-400',
        gray: 'text-gray-600 dark:text-gray-400'
      }
      return colors[this.color] || colors.blue
    }
  }
}
</script>



============================================================
FILE: src/components/Sidebar.vue
============================================================
<template>
  <div>
    <!-- Overlay for mobile -->
    <div 
      v-if="isOpen && isMobile" 
      class="fixed inset-0 bg-black bg-opacity-50 z-40 lg:hidden"
      @click="$emit('close')"
    ></div>
    
    <!-- Sidebar -->
    <aside 
      :class="[
        'fixed top-0 left-0 z-50 h-full w-64 transform transition-transform duration-300 ease-in-out',
        'bg-white dark:bg-gray-800 border-r border-gray-200 dark:border-gray-700',
        isOpen ? 'translate-x-0' : '-translate-x-full',
        'lg:translate-x-0'
      ]"
    >
      <!-- Logo Section -->
      <div class="flex items-center justify-between p-6 border-b border-gray-200 dark:border-gray-700">
        <div class="flex items-center space-x-3">
          <img 
            src="/photos/sheba-logo.png" 
            alt="Sheba Youth Foundation" 
            class="w-10 h-10 rounded-lg"
          />
          <div>
            <h1 class="text-lg font-bold text-gray-900 dark:text-white">Sheba Admin</h1>
            <p class="text-sm text-gray-500 dark:text-gray-400">Dashboard</p>
          </div>
        </div>
        <button 
          @click="$emit('close')"
          class="lg:hidden p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"
        >
          <font-awesome-icon icon="times" class="text-gray-500 dark:text-gray-400" />
        </button>
      </div>
      
      <!-- Navigation Menu -->
      <nav class="p-4 space-y-2">
        <router-link
          v-for="item in menuItems"
          :key="item.name"
          :to="item.path"
          class="flex items-center space-x-3 px-4 py-3 rounded-lg transition-colors duration-200 group"
          :class="[
            $route.name === item.name 
              ? 'bg-gradient-to-r from-cyan-500 to-orange-500 text-white shadow-lg' 
              : 'text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700'
          ]"
          @click="isMobile && $emit('close')"
        >
          <font-awesome-icon 
            :icon="item.icon" 
            class="w-5 h-5"
            :class="[
              $route.name === item.name 
                ? 'text-white' 
                : 'text-gray-500 dark:text-gray-400 group-hover:text-cyan-500'
            ]"
          />
          <span class="font-medium">{{ item.label }}</span>
        </router-link>
      </nav>
      
      <!-- Footer -->
      <div class="absolute bottom-0 left-0 right-0 p-4 border-t border-gray-200 dark:border-gray-700">
        <div class="text-center">
          <p class="text-xs text-gray-500 dark:text-gray-400">
            © 2024 Sheba Youth Foundation
          </p>
          <p class="text-xs text-gray-400 dark:text-gray-500 mt-1">
            Admin Dashboard v1.0
          </p>
        </div>
      </div>
    </aside>
  </div>
</template>

<script>
export default {
  name: 'Sidebar',
  props: {
    isOpen: {
      type: Boolean,
      default: false
    }
  },
  emits: ['toggle', 'close'],
  data() {
    return {
      isMobile: false,
      menuItems: [
        {
          name: 'dashboard',
          path: '/',
          icon: 'home',
          label: 'Dashboard'
        },
        {
          name: 'projects',
          path: '/projects',
          icon: 'project-diagram',
          label: 'Projects'
        },
        {
          name: 'publications',
          path: '/publications',
          icon: 'book',
          label: 'Publications'
        },
        {
          name: 'messages',
          path: '/messages',
          icon: 'envelope',
          label: 'Messages'
        },
        {
          name: 'settings',
          path: '/settings',
          icon: 'cog',
          label: 'Settings'
        }
      ]
    }
  },
  mounted() {
    this.checkMobile()
    window.addEventListener('resize', this.checkMobile)
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.checkMobile)
  },
  methods: {
    checkMobile() {
      this.isMobile = window.innerWidth < 1024
    }
  }
}
</script>



============================================================
FILE: src/components/StatCard.vue
============================================================
<template>
  <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700 hover:shadow-lg transition-shadow duration-300">
    <div class="flex items-center justify-between">
      <div>
        <p class="text-sm font-medium text-gray-600 dark:text-gray-400">{{ title }}</p>
        <p class="text-2xl font-bold text-gray-900 dark:text-white mt-1">{{ formattedValue }}</p>
        <div v-if="trend" class="flex items-center mt-2">
          <span :class="trendColor" class="text-sm font-medium">{{ trend }}</span>
          <span class="text-xs text-gray-500 dark:text-gray-400 ml-1">vs last month</span>
        </div>
      </div>
      <div :class="iconBgColor" class="w-12 h-12 rounded-lg flex items-center justify-center">
        <font-awesome-icon :icon="icon" :class="iconColor" class="text-xl" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'StatCard',
  props: {
    title: {
      type: String,
      required: true
    },
    value: {
      type: [Number, String],
      required: true
    },
    icon: {
      type: String,
      required: true
    },
    color: {
      type: String,
      default: 'blue'
    },
    trend: {
      type: String,
      default: null
    }
  },
  computed: {
    formattedValue() {
      if (typeof this.value === 'number') {
        return this.value.toLocaleString()
      }
      return this.value
    },
    iconBgColor() {
      const colors = {
        cyan: 'bg-cyan-100 dark:bg-cyan-900',
        purple: 'bg-purple-100 dark:bg-purple-900',
        orange: 'bg-orange-100 dark:bg-orange-900',
        green: 'bg-green-100 dark:bg-green-900',
        blue: 'bg-blue-100 dark:bg-blue-900',
        red: 'bg-red-100 dark:bg-red-900',
        gray: 'bg-gray-100 dark:bg-gray-700'
      }
      return colors[this.color] || colors.blue
    },
    iconColor() {
      const colors = {
        cyan: 'text-cyan-600 dark:text-cyan-400',
        purple: 'text-purple-600 dark:text-purple-400',
        orange: 'text-orange-600 dark:text-orange-400',
        green: 'text-green-600 dark:text-green-400',
        blue: 'text-blue-600 dark:text-blue-400',
        red: 'text-red-600 dark:text-red-400',
        gray: 'text-gray-600 dark:text-gray-400'
      }
      return colors[this.color] || colors.blue
    },
    trendColor() {
      if (!this.trend) return ''
      return this.trend.startsWith('+') 
        ? 'text-green-600 dark:text-green-400' 
        : 'text-red-600 dark:text-red-400'
    }
  }
}
</script>



============================================================
FILE: src/components/Toast.vue
============================================================
<template>
  <transition name="toast">
    <div 
      v-if="visible" 
      :class="[
        'toast',
        typeClass
      ]"
    >
      <div class="flex items-center justify-between">
        <div class="flex items-center space-x-3">
          <font-awesome-icon :icon="iconClass" class="text-lg" />
          <span>{{ message }}</span>
        </div>
        <button 
          @click="hide"
          class="ml-4 text-white hover:text-gray-200 transition-colors"
        >
          <font-awesome-icon icon="times" />
        </button>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'Toast',
  data() {
    return {
      visible: false,
      message: '',
      type: 'info',
      timeout: null
    }
  },
  computed: {
    typeClass() {
      const classes = {
        success: 'bg-green-500',
        error: 'bg-red-500',
        warning: 'bg-yellow-500',
        info: 'bg-blue-500'
      }
      return classes[this.type] || classes.info
    },
    iconClass() {
      const icons = {
        success: 'check',
        error: 'exclamation-triangle',
        warning: 'exclamation-triangle',
        info: 'info-circle'
      }
      return icons[this.type] || icons.info
    }
  },
  methods: {
    show(message, type = 'info', duration = 4000) {
      this.message = message
      this.type = type
      this.visible = true
      
      // Clear existing timeout
      if (this.timeout) {
        clearTimeout(this.timeout)
      }
      
      // Auto hide after duration
      this.timeout = setTimeout(() => {
        this.hide()
      }, duration)
    },
    hide() {
      this.visible = false
      if (this.timeout) {
        clearTimeout(this.timeout)
        this.timeout = null
      }
    }
  }
}
</script>

<style scoped>
.toast-enter-active, .toast-leave-active {
  transition: all 0.3s ease;
}

.toast-enter-from {
  transform: translateX(100%);
  opacity: 0;
}

.toast-leave-to {
  transform: translateX(100%);
  opacity: 0;
}

.toast {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 1000;
  min-width: 300px;
  padding: 16px;
  border-radius: 8px;
  color: white;
  font-weight: 500;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}
</style>



============================================================
FILE: src/components/TopNavigation.vue
============================================================
<template>
  <header class="bg-white dark:bg-gray-800 border-b border-gray-200 dark:border-gray-700 px-6 py-4">
    <div class="flex items-center justify-between">
      <!-- Left Section -->
      <div class="flex items-center space-x-4">
        <!-- Mobile Menu Button -->
        <button 
          @click="$emit('toggleSidebar')"
          class="lg:hidden p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors"
        >
          <font-awesome-icon icon="bars" class="text-gray-600 dark:text-gray-300" />
        </button>
        
        <!-- Page Title -->
        <div>
          <h2 class="text-xl font-semibold text-gray-900 dark:text-white">
            {{ currentPageTitle }}
          </h2>
          <p class="text-sm text-gray-500 dark:text-gray-400">
            {{ currentPageDescription }}
          </p>
        </div>
      </div>
      
      <!-- Right Section -->
      <div class="flex items-center space-x-4">
        <!-- Theme Toggle -->
        <button 
          @click="$emit('toggleTheme')"
          class="p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors"
          :title="isDarkMode ? 'Switch to Light Mode' : 'Switch to Dark Mode'"
        >
          <font-awesome-icon 
            :icon="isDarkMode ? 'sun' : 'moon'" 
            class="text-gray-600 dark:text-gray-300"
          />
        </button>
        
        <!-- Notifications -->
        <div class="relative">
          <button 
            class="p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors relative"
            title="Notifications"
          >
            <font-awesome-icon icon="envelope" class="text-gray-600 dark:text-gray-300" />
            <span 
              v-if="unreadMessages > 0"
              class="absolute -top-1 -right-1 bg-red-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center"
            >
              {{ unreadMessages > 9 ? '9+' : unreadMessages }}
            </span>
          </button>
        </div>
        
        <!-- User Profile -->
        <div class="flex items-center space-x-3">
          <div class="w-8 h-8 bg-gradient-to-r from-cyan-500 to-orange-500 rounded-full flex items-center justify-center">
            <font-awesome-icon icon="user" class="text-white text-sm" />
          </div>
          <div class="hidden md:block">
            <p class="text-sm font-medium text-gray-900 dark:text-white">Admin User</p>
            <p class="text-xs text-gray-500 dark:text-gray-400">Administrator</p>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: 'TopNavigation',
  props: {
    isDarkMode: {
      type: Boolean,
      default: false
    }
  },
  emits: ['toggleSidebar', 'toggleTheme'],
  computed: {
    currentPageTitle() {
      const titles = {
        'dashboard': 'Dashboard',
        'projects': 'Projects Management',
        'publications': 'Publications Management',
        'messages': 'Contact Messages',
        'settings': 'Settings'
      }
      return titles[this.$route.name] || 'Dashboard'
    },
    currentPageDescription() {
      const descriptions = {
        'dashboard': 'Overview of your content management system',
        'projects': 'Manage and organize your foundation projects',
        'publications': 'Handle reports, manuals and policy briefs',
        'messages': 'View and respond to visitor inquiries',
        'settings': 'Configure your dashboard preferences'
      }
      return descriptions[this.$route.name] || 'Welcome to your admin dashboard'
    },
    unreadMessages() {
      // This would typically come from a store or API
      const messages = JSON.parse(localStorage.getItem('contactMessages') || '[]')
      return messages.filter(msg => msg.status === 'unread').length
    }
  }
}
</script>



============================================================
FILE: src/data/mockData.js
============================================================
// Mock data for the admin dashboard

export const initialMessages = [
  {
    id: 1,
    name: "Osamah Abduljalil",
    email: "OsamahAbduljalilgmail.com",
    subject: "Partnership Inquiry",
    message: "Hello, I am interested in learning more about potential partnership opportunities with Sheba Youth Foundation.",
    date: "2024-01-15T10:30:00Z",
    status: "unread",
    priority: "medium",
    category: "Partnership"
  },
  {
    id: 2,
    name: "Doha Al-Khurasani",
    email: "DohaAl-Khurasani@gmail.com",
    subject: "Research Collaboration",
    message: "I am a graduate student researching youth empowerment in Yemen. I would like to request access to some of your published research and potentially collaborate on future studies.",
    date: "2024-01-14T14:20:00Z",
    status: "read",
    priority: "low",
    category: "Research"
  },
  {
    id: 3,
    name: "hadeel jameel",
    email: "almktaryh@gmail.com",
    subject: "Funding Opportunity",
    message: "We have reviewed your work and are impressed by your impact. We would like to discuss a potential funding opportunity for your youth programs. Please contact us at your earliest convenience.",
    date: "2024-01-13T09:15:00Z",
    status: "unread",
    priority: "high",
    category: "Funding"
  },
  {
    id: 4,
    name: "Haifaa Nabeel",
    email: "HaifaaNabeel@gmail.com",
    subject: "Community Project Proposal",
    message: "Our community in Taiz is facing several challenges that align with your foundation's mission. We would like to propose a collaborative project to address youth unemployment in our area.",
    date: "2024-01-12T16:45:00Z",
    status: "read",
    priority: "medium",
    category: "Project"
  },
  {
    id: 5,
    name: "Hassan",
    email: "Hassan@gmail.com",
    subject: "Interview Request",
    message: "I am working on a story about youth-led organizations in Yemen and would like to interview someone from your foundation about your recent projects and impact.",
    date: "2024-01-11T11:30:00Z",
    status: "unread",
    priority: "low",
    category: "Media"
  }
]

// Utility functions for data management
export const dataManager = {
  // Projects
  getProjects() {
    const stored = localStorage.getItem('projects')
    return stored ? JSON.parse(stored) : initialProjects
  },
  
  saveProjects(projects) {
    localStorage.setItem('projects', JSON.stringify(projects))
  },
  
  // Publications
  getPublications() {
    const stored = localStorage.getItem('publications')
    return stored ? JSON.parse(stored) : initialPublications
  },
  
  savePublications(publications) {
    localStorage.setItem('publications', JSON.stringify(publications))
  },
  
  // Messages
  getMessages() {
    const stored = localStorage.getItem('contactMessages')
    return stored ? JSON.parse(stored) : initialMessages
  },
  
  saveMessages(messages) {
    localStorage.setItem('contactMessages', JSON.stringify(messages))
  },
  
  // Reset all data
  resetAllData() {
    localStorage.removeItem('projects')
    localStorage.removeItem('publications')
    localStorage.removeItem('contactMessages')
  },
  
  // Initialize data if not exists
  initializeData() {
    if (!localStorage.getItem('projects')) {
      this.saveProjects(initialProjects)
    }
    if (!localStorage.getItem('publications')) {
      this.savePublications(initialPublications)
    }
    if (!localStorage.getItem('contactMessages')) {
      this.saveMessages(initialMessages)
    }
  }
}




============================================================
FILE: src/router/index.js
============================================================
import { createRouter, createWebHistory } from 'vue-router'
import Dashboard from '../views/Dashboard.vue'
import Projects from '../views/Projects.vue'
import Publications from '../views/Publications.vue'
import Messages from '../views/Messages.vue'
import Settings from '../views/Settings.vue'

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'dashboard',
      component: Dashboard,
      meta: {
        title: 'Dashboard'
      }
    },
    {
      path: '/projects',
      name: 'projects',
      component: Projects,
      meta: {
        title: 'Projects Management'
      }
    },
    {
      path: '/publications',
      name: 'publications',
      component: Publications,
      meta: {
        title: 'Publications Management'
      }
    },
    {
      path: '/messages',
      name: 'messages',
      component: Messages,
      meta: {
        title: 'Contact Messages'
      }
    },
    {
      path: '/settings',
      name: 'settings',
      component: Settings,
      meta: {
        title: 'Settings'
      }
    }
  ]
})

// Update page title
router.beforeEach((to, from, next) => {
  document.title = to.meta.title ? `${to.meta.title} - Sheba Youth Foundation Admin` : 'Sheba Youth Foundation Admin Dashboard'
  next()
})

export default router



============================================================
FILE: src/views/Dashboard.vue
============================================================
<template>
  <div class="space-y-6">
    <!-- Welcome Section -->
    <div class="bg-gradient-to-r from-cyan-500 to-orange-500 rounded-xl p-6 text-white">
      <div class="flex items-center justify-between">
        <div>
          <h1 class="text-2xl font-bold mb-2">Welcome to Sheba Admin Dashboard</h1>
          <p class="text-cyan-100">Manage your foundation's content efficiently and effectively</p>
        </div>
        <div class="hidden md:block">
          <img 
            src="/photos/sheba-logo.png" 
            alt="Sheba Logo" 
            class="w-16 h-16 rounded-lg bg-white/20 p-2"
          />
        </div>
      </div>
    </div>

    <!-- Statistics Cards -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
      <StatCard
        title="Total Projects"
        :value="stats.totalProjects"
        icon="project-diagram"
        color="cyan"
        :trend="stats.projectsTrend"
      />
      <StatCard
        title="Publications"
        :value="stats.totalPublications"
        icon="book"
        color="purple"
        :trend="stats.publicationsTrend"
      />
      <StatCard
        title="New Messages"
        :value="stats.unreadMessages"
        icon="envelope"
        color="orange"
        :trend="stats.messagesTrend"
      />
      <StatCard
        title="Total Downloads"
        :value="stats.totalDownloads"
        icon="download"
        color="green"
        :trend="stats.downloadsTrend"
      />
    </div>

    <!-- Charts Section -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
      <!-- Projects by Status -->
      <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
        <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-4">Projects by Status</h3>
        <div class="space-y-4">
          <div v-for="status in projectsByStatus" :key="status.name" class="flex items-center justify-between">
            <div class="flex items-center space-x-3">
              <div :class="status.color" class="w-4 h-4 rounded-full"></div>
              <span class="text-gray-700 dark:text-gray-300">{{ status.name }}</span>
            </div>
            <div class="flex items-center space-x-2">
              <span class="font-semibold text-gray-900 dark:text-white">{{ status.count }}</span>
              <div class="w-20 bg-gray-200 dark:bg-gray-700 rounded-full h-2">
                <div 
                  :class="status.color" 
                  class="h-2 rounded-full transition-all duration-300"
                  :style="{ width: `${(status.count / stats.totalProjects) * 100}%` }"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Recent Activity -->
      <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
        <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-4">Recent Activity</h3>
        <div class="space-y-4">
          <div v-for="activity in recentActivities" :key="activity.id" class="flex items-start space-x-3">
            <div :class="activity.iconBg" class="w-8 h-8 rounded-full flex items-center justify-center flex-shrink-0">
              <font-awesome-icon :icon="activity.icon" :class="activity.iconColor" class="text-sm" />
            </div>
            <div class="flex-1 min-w-0">
              <p class="text-sm text-gray-900 dark:text-white">{{ activity.title }}</p>
              <p class="text-xs text-gray-500 dark:text-gray-400">{{ formatDate(activity.date) }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Quick Actions -->
    <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
      <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-4">Quick Actions</h3>
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
        <QuickActionButton
          title="Add New Project"
          description="Create a new project entry"
          icon="plus"
          color="cyan"
          @click="$router.push('/projects')"
        />
        <QuickActionButton
          title="Add Publication"
          description="Upload a new publication"
          icon="book"
          color="purple"
          @click="$router.push('/publications')"
        />
        <QuickActionButton
          title="View Messages"
          description="Check visitor messages"
          icon="envelope"
          color="orange"
          @click="$router.push('/messages')"
        />
        <QuickActionButton
          title="Settings"
          description="Configure dashboard"
          icon="cog"
          color="gray"
          @click="$router.push('/settings')"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { dataManager } from '../data/mockData.js'
import StatCard from '../components/StatCard.vue'
import QuickActionButton from '../components/QuickActionButton.vue'

export default {
  name: 'Dashboard',
  components: {
    StatCard,
    QuickActionButton
  },
  data() {
    return {
      projects: [],
      publications: [],
      messages: []
    }
  },
  computed: {
    stats() {
      const unreadMessages = this.messages.filter(msg => msg.status === 'unread').length
      const totalDownloads = this.publications.reduce((sum, pub) => sum + (pub.downloadCount || 0), 0)
      
      return {
        totalProjects: this.projects.length,
        totalPublications: this.publications.length,
        unreadMessages,
        totalDownloads,
        projectsTrend: '+12%',
        publicationsTrend: '+8%',
        messagesTrend: '+5%',
        downloadsTrend: '+23%'
      }
    },
    projectsByStatus() {
      const statusCounts = this.projects.reduce((acc, project) => {
        acc[project.status] = (acc[project.status] || 0) + 1
        return acc
      }, {})
      
      return [
        { name: 'Active', count: statusCounts.Active || 0, color: 'bg-green-500' },
        { name: 'Completed', count: statusCounts.Completed || 0, color: 'bg-blue-500' },
        { name: 'Planning', count: statusCounts.Planning || 0, color: 'bg-yellow-500' },
        { name: 'On Hold', count: statusCounts['On Hold'] || 0, color: 'bg-red-500' }
      ]
    },
    recentActivities() {
      const activities = []
      
      // Add recent projects
      this.projects.slice(0, 2).forEach(project => {
        activities.push({
          id: `project-${project.id}`,
          title: `Project "${project.title}" updated`,
          date: new Date(project.startDate),
          icon: 'project-diagram',
          iconBg: 'bg-cyan-100 dark:bg-cyan-900',
          iconColor: 'text-cyan-600 dark:text-cyan-400'
        })
      })
      
      // Add recent publications
      this.publications.slice(0, 2).forEach(publication => {
        activities.push({
          id: `publication-${publication.id}`,
          title: `Publication "${publication.title}" published`,
          date: new Date(publication.publishDate),
          icon: 'book',
          iconBg: 'bg-purple-100 dark:bg-purple-900',
          iconColor: 'text-purple-600 dark:text-purple-400'
        })
      })
      
      // Add recent messages
      this.messages.slice(0, 1).forEach(message => {
        activities.push({
          id: `message-${message.id}`,
          title: `New message from ${message.name}`,
          date: new Date(message.date),
          icon: 'envelope',
          iconBg: 'bg-orange-100 dark:bg-orange-900',
          iconColor: 'text-orange-600 dark:text-orange-400'
        })
      })
      
      return activities.sort((a, b) => new Date(b.date) - new Date(a.date)).slice(0, 5)
    }
  },
  mounted() {
    this.loadData()
  },
  methods: {
    loadData() {
      this.projects = dataManager.getProjects()
      this.publications = dataManager.getPublications()
      this.messages = dataManager.getMessages()
    },
    formatDate(date) {
      return new Date(date).toLocaleDateString('en-US', {
        month: 'short',
        day: 'numeric',
        year: 'numeric'
      })
    }
  }
}
</script>



============================================================
FILE: src/views/Messages.vue
============================================================
<template>
  <div class="space-y-6">
    <!-- Header Section -->
    <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
      <div>
        <h1 class="text-2xl font-bold text-gray-900 dark:text-white">Contact Messages</h1>
        <p class="text-gray-600 dark:text-gray-400">Manage visitor inquiries and feedback</p>
      </div>
      <div class="flex items-center space-x-3">
        <button 
          @click="markAllAsRead"
          class="inline-flex items-center px-4 py-2 bg-gradient-to-r from-green-500 to-teal-500 text-white rounded-lg hover:shadow-lg transition-all duration-300"
        >
          <font-awesome-icon icon="check-double" class="mr-2" />
          Mark All Read
        </button>
        <button 
          @click="deleteAllRead"
          class="inline-flex items-center px-4 py-2 bg-gradient-to-r from-red-500 to-pink-500 text-white rounded-lg hover:shadow-lg transition-all duration-300"
        >
          <font-awesome-icon icon="trash" class="mr-2" />
          Delete Read
        </button>
      </div>
    </div>

    <!-- Stats Cards -->
    <div class="grid grid-cols-1 md:grid-cols-4 gap-6">
      <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
        <div class="flex items-center justify-between">
          <div>
            <p class="text-sm font-medium text-gray-600 dark:text-gray-400">Total Messages</p>
            <p class="text-2xl font-bold text-gray-900 dark:text-white">{{ messages.length }}</p>
          </div>
          <div class="w-12 h-12 bg-blue-100 dark:bg-blue-900 rounded-lg flex items-center justify-center">
            <font-awesome-icon icon="envelope" class="text-blue-600 dark:text-blue-400 text-xl" />
          </div>
        </div>
      </div>

      <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
        <div class="flex items-center justify-between">
          <div>
            <p class="text-sm font-medium text-gray-600 dark:text-gray-400">Unread</p>
            <p class="text-2xl font-bold text-gray-900 dark:text-white">{{ unreadCount }}</p>
          </div>
          <div class="w-12 h-12 bg-orange-100 dark:bg-orange-900 rounded-lg flex items-center justify-center">
            <font-awesome-icon icon="envelope-open" class="text-orange-600 dark:text-orange-400 text-xl" />
          </div>
        </div>
      </div>

      <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
        <div class="flex items-center justify-between">
          <div>
            <p class="text-sm font-medium text-gray-600 dark:text-gray-400">Replied</p>
            <p class="text-2xl font-bold text-gray-900 dark:text-white">{{ repliedCount }}</p>
          </div>
          <div class="w-12 h-12 bg-green-100 dark:bg-green-900 rounded-lg flex items-center justify-center">
            <font-awesome-icon icon="reply" class="text-green-600 dark:text-green-400 text-xl" />
          </div>
        </div>
      </div>

      <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
        <div class="flex items-center justify-between">
          <div>
            <p class="text-sm font-medium text-gray-600 dark:text-gray-400">This Week</p>
            <p class="text-2xl font-bold text-gray-900 dark:text-white">{{ thisWeekCount }}</p>
          </div>
          <div class="w-12 h-12 bg-purple-100 dark:bg-purple-900 rounded-lg flex items-center justify-center">
            <font-awesome-icon icon="calendar-week" class="text-purple-600 dark:text-purple-400 text-xl" />
          </div>
        </div>
      </div>
    </div>

    <!-- Filters and Search -->
    <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
        <!-- Search -->
        <div class="relative">
          <font-awesome-icon icon="search" class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" />
          <input
            v-model="searchQuery"
            type="text"
            placeholder="Search messages..."
            class="w-full pl-10 pr-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
          />
        </div>
        
        <!-- Status Filter -->
        <select 
          v-model="statusFilter"
          class="px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
        >
          <option value="">All Messages</option>
          <option value="unread">Unread</option>
          <option value="read">Read</option>
          <option value="replied">Replied</option>
        </select>
        
        <!-- Date Filter -->
        <select 
          v-model="dateFilter"
          class="px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
        >
          <option value="">All Time</option>
          <option value="today">Today</option>
          <option value="week">This Week</option>
          <option value="month">This Month</option>
        </select>
        
        <!-- Clear Filters -->
        <button 
          @click="clearFilters"
          class="px-4 py-2 text-gray-600 dark:text-gray-400 border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
        >
          Clear Filters
        </button>
      </div>
    </div>

    <!-- Messages List -->
    <div class="bg-white dark:bg-gray-800 rounded-xl border border-gray-200 dark:border-gray-700 overflow-hidden">
      <div class="divide-y divide-gray-200 dark:divide-gray-700">
        <div 
          v-for="message in filteredMessages" 
          :key="message.id"
          :class="[
            'p-6 hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors cursor-pointer',
            !message.isRead ? 'bg-blue-50 dark:bg-blue-900/20 border-l-4 border-blue-500' : ''
          ]"
          @click="viewMessage(message)"
        >
          <div class="flex items-start justify-between">
            <div class="flex-1">
              <!-- Message Header -->
              <div class="flex items-center space-x-3 mb-2">
                <div class="w-10 h-10 bg-gradient-to-r from-blue-500 to-purple-500 rounded-full flex items-center justify-center text-white font-semibold">
                  {{ message.name.charAt(0).toUpperCase() }}
                </div>
                <div class="flex-1">
                  <div class="flex items-center space-x-2">
                    <h3 class="text-lg font-semibold text-gray-900 dark:text-white">{{ message.name }}</h3>
                    <span v-if="!message.isRead" class="w-2 h-2 bg-blue-500 rounded-full"></span>
                  </div>
                  <p class="text-sm text-gray-600 dark:text-gray-400">{{ message.email }}</p>
                </div>
              </div>

              <!-- Message Content -->
              <div class="mb-3">
                <h4 class="text-base font-medium text-gray-900 dark:text-white mb-1">{{ message.subject }}</h4>
                <p class="text-gray-600 dark:text-gray-400 line-clamp-2">{{ message.message }}</p>
              </div>

              <!-- Message Meta -->
              <div class="flex items-center space-x-4 text-sm text-gray-500 dark:text-gray-400">
                <div class="flex items-center space-x-1">
                  <font-awesome-icon icon="clock" />
                  <span>{{ formatDate(message.date) }}</span>
                </div>
                <div v-if="message.phone" class="flex items-center space-x-1">
                  <font-awesome-icon icon="phone" />
                  <span>{{ message.phone }}</span>
                </div>
                <div v-if="message.isReplied" class="flex items-center space-x-1 text-green-600 dark:text-green-400">
                  <font-awesome-icon icon="reply" />
                  <span>Replied</span>
                </div>
              </div>
            </div>

            <!-- Actions -->
            <div class="flex items-center space-x-2 ml-4">
              <button 
                @click.stop="toggleRead(message)"
                :class="[
                  'p-2 rounded-lg transition-colors',
                  message.isRead 
                    ? 'text-gray-400 hover:text-gray-600 dark:text-gray-500 dark:hover:text-gray-400' 
                    : 'text-blue-600 hover:text-blue-800 dark:text-blue-400 dark:hover:text-blue-300'
                ]"
                :title="message.isRead ? 'Mark as Unread' : 'Mark as Read'"
              >
                <font-awesome-icon :icon="message.isRead ? 'envelope-open' : 'envelope'" />
              </button>
              <button 
                @click.stop="replyToMessage(message)"
                class="p-2 text-green-600 hover:text-green-800 dark:text-green-400 dark:hover:text-green-300 rounded-lg transition-colors"
                title="Reply"
              >
                <font-awesome-icon icon="reply" />
              </button>
              <button 
                @click.stop="deleteMessage(message)"
                class="p-2 text-red-600 hover:text-red-800 dark:text-red-400 dark:hover:text-red-300 rounded-lg transition-colors"
                title="Delete"
              >
                <font-awesome-icon icon="trash" />
              </button>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Empty State -->
      <div v-if="filteredMessages.length === 0" class="text-center py-12">
        <font-awesome-icon icon="inbox" class="text-6xl text-gray-300 dark:text-gray-600 mb-4" />
        <h3 class="text-lg font-medium text-gray-900 dark:text-white mb-2">No messages found</h3>
        <p class="text-gray-500 dark:text-gray-400">Try adjusting your search or filter criteria</p>
      </div>
    </div>

    <!-- Pagination -->
    <div v-if="filteredMessages.length > 0" class="flex items-center justify-between">
      <div class="text-sm text-gray-700 dark:text-gray-300">
        Showing {{ filteredMessages.length }} of {{ messages.length }} messages
      </div>
    </div>

    <!-- View Message Modal -->
    <MessageViewModal
      v-if="showViewModal"
      :message="selectedMessage"
      @close="closeViewModal"
      @reply="replyToMessage"
      @delete="deleteMessage"
      @toggleRead="toggleRead"
    />

    <!-- Reply Modal -->
    <MessageReplyModal
      v-if="showReplyModal"
      :message="selectedMessage"
      @close="closeReplyModal"
      @send="sendReply"
    />

    <!-- Delete Confirmation Modal -->
    <ConfirmModal
      v-if="showDeleteModal"
      title="Delete Message"
      :message="`Are you sure you want to delete the message from '${selectedMessage?.name}'? This action cannot be undone.`"
      @confirm="confirmDelete"
      @cancel="closeDeleteModal"
    />
  </div>
</template>

<script>
import { dataManager } from '../data/mockData.js'
import MessageViewModal from '../components/MessageViewModal.vue'
import MessageReplyModal from '../components/MessageReplyModal.vue'
import ConfirmModal from '../components/ConfirmModal.vue'

export default {
  name: 'Messages',
  components: {
    MessageViewModal,
    MessageReplyModal,
    ConfirmModal
  },
  inject: ['showToast'],
  data() {
    return {
      messages: [],
      searchQuery: '',
      statusFilter: '',
      dateFilter: '',
      showViewModal: false,
      showReplyModal: false,
      showDeleteModal: false,
      selectedMessage: null
    }
  },
  computed: {
    filteredMessages() {
      let filtered = this.messages

      // Search filter
      if (this.searchQuery) {
        const query = this.searchQuery.toLowerCase()
        filtered = filtered.filter(message => 
          message.name.toLowerCase().includes(query) ||
          message.email.toLowerCase().includes(query) ||
          message.subject.toLowerCase().includes(query) ||
          message.message.toLowerCase().includes(query)
        )
      }

      // Status filter
      if (this.statusFilter) {
        if (this.statusFilter === 'unread') {
          filtered = filtered.filter(message => !message.isRead)
        } else if (this.statusFilter === 'read') {
          filtered = filtered.filter(message => message.isRead && !message.isReplied)
        } else if (this.statusFilter === 'replied') {
          filtered = filtered.filter(message => message.isReplied)
        }
      }

      // Date filter
      if (this.dateFilter) {
        const now = new Date()
        const today = new Date(now.getFullYear(), now.getMonth(), now.getDate())
        const weekAgo = new Date(today.getTime() - 7 * 24 * 60 * 60 * 1000)
        const monthAgo = new Date(today.getTime() - 30 * 24 * 60 * 60 * 1000)

        filtered = filtered.filter(message => {
          const messageDate = new Date(message.date)
          if (this.dateFilter === 'today') {
            return messageDate >= today
          } else if (this.dateFilter === 'week') {
            return messageDate >= weekAgo
          } else if (this.dateFilter === 'month') {
            return messageDate >= monthAgo
          }
          return true
        })
      }

      // Sort by date (newest first)
      return filtered.sort((a, b) => new Date(b.date) - new Date(a.date))
    },

    unreadCount() {
      return this.messages.filter(message => !message.isRead).length
    },

    repliedCount() {
      return this.messages.filter(message => message.isReplied).length
    },

    thisWeekCount() {
      const weekAgo = new Date()
      weekAgo.setDate(weekAgo.getDate() - 7)
      return this.messages.filter(message => new Date(message.date) >= weekAgo).length
    }
  },
  mounted() {
    this.loadMessages()
    dataManager.initializeData()
  },
  methods: {
    loadMessages() {
      this.messages = dataManager.getMessages()
    },
    
    clearFilters() {
      this.searchQuery = ''
      this.statusFilter = ''
      this.dateFilter = ''
    },
    
    formatDate(dateString) {
      const date = new Date(dateString)
      const now = new Date()
      const diffTime = Math.abs(now - date)
      const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24))
      
      if (diffDays === 1) {
        return 'Today'
      } else if (diffDays === 2) {
        return 'Yesterday'
      } else if (diffDays <= 7) {
        return `${diffDays - 1} days ago`
      } else {
        return date.toLocaleDateString('en-US', {
          year: 'numeric',
          month: 'short',
          day: 'numeric'
        })
      }
    },
    
    viewMessage(message) {
      this.selectedMessage = message
      this.showViewModal = true
      
      // Mark as read when viewed
      if (!message.isRead) {
        this.toggleRead(message)
      }
    },
    
    toggleRead(message) {
      const index = this.messages.findIndex(m => m.id === message.id)
      if (index !== -1) {
        this.messages[index].isRead = !this.messages[index].isRead
        dataManager.saveMessages(this.messages)
        
        const status = this.messages[index].isRead ? 'read' : 'unread'
        this.showToast(`Message marked as ${status}`, 'success')
      }
    },
    
    replyToMessage(message) {
      this.selectedMessage = message
      this.showReplyModal = true
      this.closeViewModal()
    },
    
    deleteMessage(message) {
      this.selectedMessage = message
      this.showDeleteModal = true
      this.closeViewModal()
    },
    
    markAllAsRead() {
      this.messages.forEach(message => {
        message.isRead = true
      })
      dataManager.saveMessages(this.messages)
      this.showToast('All messages marked as read', 'success')
    },
    
    deleteAllRead() {
      const readMessages = this.messages.filter(message => message.isRead)
      if (readMessages.length === 0) {
        this.showToast('No read messages to delete', 'info')
        return
      }
      
      this.messages = this.messages.filter(message => !message.isRead)
      dataManager.saveMessages(this.messages)
      this.showToast(`${readMessages.length} read messages deleted`, 'success')
    },
    
    closeViewModal() {
      this.showViewModal = false
      this.selectedMessage = null
    },
    
    closeReplyModal() {
      this.showReplyModal = false
      this.selectedMessage = null
    },
    
    closeDeleteModal() {
      this.showDeleteModal = false
      this.selectedMessage = null
    },
    
    sendReply(replyData) {
      // Mark message as replied
      const index = this.messages.findIndex(m => m.id === this.selectedMessage.id)
      if (index !== -1) {
        this.messages[index].isReplied = true
        this.messages[index].isRead = true
        this.messages[index].replyDate = new Date().toISOString()
        this.messages[index].replyMessage = replyData.message
        dataManager.saveMessages(this.messages)
      }
      
      this.showToast('Reply sent successfully!', 'success')
      this.closeReplyModal()
    },
    
    confirmDelete() {
      const index = this.messages.findIndex(m => m.id === this.selectedMessage.id)
      if (index !== -1) {
        this.messages.splice(index, 1)
        dataManager.saveMessages(this.messages)
        this.showToast('Message deleted successfully!', 'success')
      }
      this.closeDeleteModal()
    }
  }
}
</script>

<style scoped>
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>



============================================================
FILE: src/views/Projects.vue
============================================================
<template>
  <div class="space-y-6">
    <!-- Header Section -->
    <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
      <div>
        <h1 class="text-2xl font-bold text-gray-900 dark:text-white">Projects Management</h1>
        <p class="text-gray-600 dark:text-gray-400">Manage and organize your foundation projects</p>
      </div>
      <button 
        @click="openAddModal"
        class="inline-flex items-center px-4 py-2 bg-gradient-to-r from-cyan-500 to-orange-500 text-white rounded-lg hover:shadow-lg transition-all duration-300"
      >
        <font-awesome-icon icon="plus" class="mr-2" />
        Add New Project
      </button>
    </div>

    <!-- Filters and Search -->
    <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
        <!-- Search -->
        <div class="relative">
          <font-awesome-icon icon="search" class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" />
          <input
            v-model="searchQuery"
            type="text"
            placeholder="Search projects..."
            class="w-full pl-10 pr-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
          />
        </div>
        
        <!-- Status Filter -->
        <select 
          v-model="statusFilter"
          class="px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
        >
          <option value="">All Status</option>
          <option value="Active">Active</option>
          <option value="Completed">Completed</option>
          <option value="Planning">Planning</option>
          <option value="On Hold">On Hold</option>
        </select>
        
        <!-- Category Filter -->
        <select 
          v-model="categoryFilter"
          class="px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-cyan-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
        >
          <option value="">All Categories</option>
          <option value="Community Development">Community Development</option>
          <option value="Youth Development">Youth Development</option>
          <option value="Capacity Building">Capacity Building</option>
          <option value="Media Development">Media Development</option>
        </select>
        
        <!-- Clear Filters -->
        <button 
          @click="clearFilters"
          class="px-4 py-2 text-gray-600 dark:text-gray-400 border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
        >
          Clear Filters
        </button>
      </div>
    </div>

    <!-- Projects Table -->
    <div class="bg-white dark:bg-gray-800 rounded-xl border border-gray-200 dark:border-gray-700 overflow-hidden">
      <div class="overflow-x-auto">
        <table class="w-full">
          <thead class="bg-gray-50 dark:bg-gray-700">
            <tr>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-400 uppercase tracking-wider">
                Project
              </th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-400 uppercase tracking-wider">
                Location
              </th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-400 uppercase tracking-wider">
                Status
              </th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-400 uppercase tracking-wider">
                Budget
              </th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-400 uppercase tracking-wider">
                Actions
              </th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-200 dark:divide-gray-700">
            <tr 
              v-for="project in filteredProjects" 
              :key="project.id"
              class="hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
            >
              <td class="px-6 py-4">
                <div class="flex items-center">
                  <img 
                    :src="project.image" 
                    :alt="project.title"
                    class="w-12 h-12 rounded-lg object-cover mr-4"
                  />
                  <div>
                    <div class="text-sm font-medium text-gray-900 dark:text-white">
                      {{ project.title }}
                    </div>
                    <div class="text-sm text-gray-500 dark:text-gray-400">
                      {{ project.beneficiaries }}
                    </div>
                  </div>
                </div>
              </td>
              <td class="px-6 py-4 text-sm text-gray-900 dark:text-white">
                <div class="flex items-center">
                  <font-awesome-icon icon="map-marker-alt" class="text-gray-400 mr-2" />
                  {{ project.location }}
                </div>
              </td>
              <td class="px-6 py-4">
                <span :class="getStatusClass(project.status)" class="px-2 py-1 text-xs font-medium rounded-full">
                  {{ project.status }}
                </span>
              </td>
              <td class="px-6 py-4 text-sm text-gray-900 dark:text-white">
                ${{ project.budget?.toLocaleString() || 'N/A' }}
              </td>
              <td class="px-6 py-4 text-sm font-medium">
                <div class="flex items-center space-x-2">
                  <button 
                    @click="viewProject(project)"
                    class="text-cyan-600 hover:text-cyan-900 dark:text-cyan-400 dark:hover:text-cyan-300"
                    title="View Details"
                  >
                    <font-awesome-icon icon="eye" />
                  </button>
                  <button 
                    @click="editProject(project)"
                    class="text-orange-600 hover:text-orange-900 dark:text-orange-400 dark:hover:text-orange-300"
                    title="Edit Project"
                  >
                    <font-awesome-icon icon="edit" />
                  </button>
                  <button 
                    @click="deleteProject(project)"
                    class="text-red-600 hover:text-red-900 dark:text-red-400 dark:hover:text-red-300"
                    title="Delete Project"
                  >
                    <font-awesome-icon icon="trash" />
                  </button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      
      <!-- Empty State -->
      <div v-if="filteredProjects.length === 0" class="text-center py-12">
        <font-awesome-icon icon="project-diagram" class="text-6xl text-gray-300 dark:text-gray-600 mb-4" />
        <h3 class="text-lg font-medium text-gray-900 dark:text-white mb-2">No projects found</h3>
        <p class="text-gray-500 dark:text-gray-400">Try adjusting your search or filter criteria</p>
      </div>
    </div>

    <!-- Pagination -->
    <div v-if="filteredProjects.length > 0" class="flex items-center justify-between">
      <div class="text-sm text-gray-700 dark:text-gray-300">
        Showing {{ filteredProjects.length }} of {{ projects.length }} projects
      </div>
    </div>

    <!-- Add/Edit Modal -->
    <ProjectModal
      v-if="showModal"
      :project="selectedProject"
      :isEdit="isEditMode"
      @close="closeModal"
      @save="saveProject"
    />

    <!-- View Modal -->
    <ProjectViewModal
      v-if="showViewModal"
      :project="selectedProject"
      @close="closeViewModal"
    />

    <!-- Delete Confirmation Modal -->
    <ConfirmModal
      v-if="showDeleteModal"
      title="Delete Project"
      :message="`Are you sure you want to delete '${selectedProject?.title}'? This action cannot be undone.`"
      @confirm="confirmDelete"
      @cancel="closeDeleteModal"
    />
  </div>
</template>

<script>
import { dataManager } from '../data/mockData.js'
import ProjectModal from '../components/ProjectModal.vue'
import ProjectViewModal from '../components/ProjectViewModal.vue'
import ConfirmModal from '../components/ConfirmModal.vue'

export default {
  name: 'Projects',
  components: {
    ProjectModal,
    ProjectViewModal,
    ConfirmModal
  },
  inject: ['showToast'],
  data() {
    return {
      projects: [],
      searchQuery: '',
      statusFilter: '',
      categoryFilter: '',
      showModal: false,
      showViewModal: false,
      showDeleteModal: false,
      selectedProject: null,
      isEditMode: false
    }
  },
  computed: {
    filteredProjects() {
      let filtered = this.projects

      // Search filter
      if (this.searchQuery) {
        const query = this.searchQuery.toLowerCase()
        filtered = filtered.filter(project => 
          project.title.toLowerCase().includes(query) ||
          project.description.toLowerCase().includes(query) ||
          project.location.toLowerCase().includes(query)
        )
      }

      // Status filter
      if (this.statusFilter) {
        filtered = filtered.filter(project => project.status === this.statusFilter)
      }

      // Category filter
      if (this.categoryFilter) {
        filtered = filtered.filter(project => project.category === this.categoryFilter)
      }

      return filtered
    }
  },
  mounted() {
    this.loadProjects()
    dataManager.initializeData()
  },
  methods: {
    loadProjects() {
      this.projects = dataManager.getProjects()
    },
    
    clearFilters() {
      this.searchQuery = ''
      this.statusFilter = ''
      this.categoryFilter = ''
    },
    
    getStatusClass(status) {
      const classes = {
        'Active': 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300',
        'Completed': 'bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-300',
        'Planning': 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900 dark:text-yellow-300',
        'On Hold': 'bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-300'
      }
      return classes[status] || 'bg-gray-100 text-gray-800 dark:bg-gray-900 dark:text-gray-300'
    },
    
    openAddModal() {
      this.selectedProject = null
      this.isEditMode = false
      this.showModal = true
    },
    
    viewProject(project) {
      this.selectedProject = project
      this.showViewModal = true
    },
    
    editProject(project) {
      this.selectedProject = { ...project }
      this.isEditMode = true
      this.showModal = true
    },
    
    deleteProject(project) {
      this.selectedProject = project
      this.showDeleteModal = true
    },
    
    closeModal() {
      this.showModal = false
      this.selectedProject = null
      this.isEditMode = false
    },
    
    closeViewModal() {
      this.showViewModal = false
      this.selectedProject = null
    },
    
    closeDeleteModal() {
      this.showDeleteModal = false
      this.selectedProject = null
    },
    
    saveProject(projectData) {
      if (this.isEditMode) {
        // Update existing project
        const index = this.projects.findIndex(p => p.id === projectData.id)
        if (index !== -1) {
          this.projects[index] = projectData
          this.showToast('Project updated successfully!', 'success')
        }
      } else {
        // Add new project
        const newProject = {
          ...projectData,
          id: Math.max(...this.projects.map(p => p.id), 0) + 1
        }
        this.projects.push(newProject)
        this.showToast('Project added successfully!', 'success')
      }
      
      dataManager.saveProjects(this.projects)
      this.closeModal()
    },
    
    confirmDelete() {
      const index = this.projects.findIndex(p => p.id === this.selectedProject.id)
      if (index !== -1) {
        this.projects.splice(index, 1)
        dataManager.saveProjects(this.projects)
        this.showToast('Project deleted successfully!', 'success')
      }
      this.closeDeleteModal()
    }
  }
}
</script>



============================================================
FILE: src/views/Publications.vue
============================================================
<template>
  <div class="space-y-6">
    <!-- Header Section -->
    <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
      <div>
        <h1 class="text-2xl font-bold text-gray-900 dark:text-white">Publications Management</h1>
        <p class="text-gray-600 dark:text-gray-400">Handle reports, manuals and policy briefs</p>
      </div>
      <button 
        @click="openAddModal"
        class="inline-flex items-center px-4 py-2 bg-gradient-to-r from-purple-500 to-pink-500 text-white rounded-lg hover:shadow-lg transition-all duration-300"
      >
        <font-awesome-icon icon="plus" class="mr-2" />
        Add New Publication
      </button>
    </div>

    <!-- Filters and Search -->
    <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
        <!-- Search -->
        <div class="relative">
          <font-awesome-icon icon="search" class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" />
          <input
            v-model="searchQuery"
            type="text"
            placeholder="Search publications..."
            class="w-full pl-10 pr-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
          />
        </div>
        
        <!-- Type Filter -->
        <select 
          v-model="typeFilter"
          class="px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
        >
          <option value="">All Types</option>
          <option value="Policy Brief">Policy Brief</option>
          <option value="Manual">Manual</option>
          <option value="Report">Report</option>
          <option value="Guide">Guide</option>
        </select>
        
        <!-- Year Filter -->
        <select 
          v-model="yearFilter"
          class="px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent dark:bg-gray-700 dark:text-white"
        >
          <option value="">All Years</option>
          <option value="2024">2024</option>
          <option value="2023">2023</option>
          <option value="2022">2022</option>
          <option value="2021">2021</option>
        </select>
        
        <!-- Clear Filters -->
        <button 
          @click="clearFilters"
          class="px-4 py-2 text-gray-600 dark:text-gray-400 border border-gray-300 dark:border-gray-600 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
        >
          Clear Filters
        </button>
      </div>
    </div>

    <!-- Publications Grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div 
        v-for="publication in filteredPublications" 
        :key="publication.id"
        class="bg-white dark:bg-gray-800 rounded-xl border border-gray-200 dark:border-gray-700 overflow-hidden hover:shadow-lg transition-all duration-300"
      >
        <!-- Cover Image -->
        <div class="relative h-48 bg-gray-100 dark:bg-gray-700">
          <img 
            :src="publication.cover" 
            :alt="publication.title"
            class="w-full h-full object-cover"
          />
          <div class="absolute top-3 right-3">
            <span :class="getTypeClass(publication.type)" class="px-2 py-1 text-xs font-medium rounded-full">
              {{ publication.type }}
            </span>
          </div>
        </div>

        <!-- Content -->
        <div class="p-6">
          <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-2 line-clamp-2">
            {{ publication.title }}
          </h3>
          <p v-if="publication.subtitle" class="text-sm text-gray-600 dark:text-gray-400 mb-3 line-clamp-2">
            {{ publication.subtitle }}
          </p>
          <p class="text-sm text-gray-500 dark:text-gray-400 mb-4 line-clamp-3">
            {{ publication.description }}
          </p>

          <!-- Publication Info -->
          <div class="space-y-2 mb-4">
            <div class="flex items-center text-sm text-gray-600 dark:text-gray-400">
              <font-awesome-icon icon="calendar" class="mr-2" />
              {{ publication.year }}
            </div>
            <div v-if="publication.pages" class="flex items-center text-sm text-gray-600 dark:text-gray-400">
              <font-awesome-icon icon="file-alt" class="mr-2" />
              {{ publication.pages }} pages
            </div>
            <div v-if="publication.downloadCount" class="flex items-center text-sm text-gray-600 dark:text-gray-400">
              <font-awesome-icon icon="download" class="mr-2" />
              {{ publication.downloadCount }} downloads
            </div>
          </div>

          <!-- Tags -->
          <div v-if="publication.tags && publication.tags.length" class="flex flex-wrap gap-1 mb-4">
            <span 
              v-for="tag in publication.tags.slice(0, 3)" 
              :key="tag"
              class="px-2 py-1 text-xs bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-400 rounded-full"
            >
              {{ tag }}
            </span>
            <span 
              v-if="publication.tags.length > 3"
              class="px-2 py-1 text-xs bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-400 rounded-full"
            >
              +{{ publication.tags.length - 3 }}
            </span>
          </div>

          <!-- Actions -->
          <div class="flex items-center justify-between">
            <div class="flex items-center space-x-2">
              <button 
                @click="viewPublication(publication)"
                class="text-purple-600 hover:text-purple-900 dark:text-purple-400 dark:hover:text-purple-300"
                title="View Details"
              >
                <font-awesome-icon icon="eye" />
              </button>
              <button 
                @click="editPublication(publication)"
                class="text-orange-600 hover:text-orange-900 dark:text-orange-400 dark:hover:text-orange-300"
                title="Edit Publication"
              >
                <font-awesome-icon icon="edit" />
              </button>
              <button 
                @click="deletePublication(publication)"
                class="text-red-600 hover:text-red-900 dark:text-red-400 dark:hover:text-red-300"
                title="Delete Publication"
              >
                <font-awesome-icon icon="trash" />
              </button>
            </div>
            <button 
              v-if="publication.pdfUrl"
              @click="downloadPDF(publication)"
              class="text-sm text-gray-600 dark:text-gray-400 hover:text-purple-600 dark:hover:text-purple-400 transition-colors"
              title="Download PDF"
            >
              <font-awesome-icon icon="download" class="mr-1" />
              PDF
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Empty State -->
    <div v-if="filteredPublications.length === 0" class="text-center py-12">
      <font-awesome-icon icon="book" class="text-6xl text-gray-300 dark:text-gray-600 mb-4" />
      <h3 class="text-lg font-medium text-gray-900 dark:text-white mb-2">No publications found</h3>
      <p class="text-gray-500 dark:text-gray-400">Try adjusting your search or filter criteria</p>
    </div>

    <!-- Pagination -->
    <div v-if="filteredPublications.length > 0" class="flex items-center justify-between">
      <div class="text-sm text-gray-700 dark:text-gray-300">
        Showing {{ filteredPublications.length }} of {{ publications.length }} publications
      </div>
    </div>

    <!-- Add/Edit Modal -->
    <PublicationModal
      v-if="showModal"
      :publication="selectedPublication"
      :isEdit="isEditMode"
      @close="closeModal"
      @save="savePublication"
    />

    <!-- View Modal -->
    <PublicationViewModal
      v-if="showViewModal"
      :publication="selectedPublication"
      @close="closeViewModal"
    />

    <!-- Delete Confirmation Modal -->
    <ConfirmModal
      v-if="showDeleteModal"
      title="Delete Publication"
      :message="`Are you sure you want to delete '${selectedPublication?.title}'? This action cannot be undone.`"
      @confirm="confirmDelete"
      @cancel="closeDeleteModal"
    />
  </div>
</template>

<script>
import { dataManager } from '../data/mockData.js'
import PublicationModal from '../components/PublicationModal.vue'
import PublicationViewModal from '../components/PublicationViewModal.vue'
import ConfirmModal from '../components/ConfirmModal.vue'

export default {
  name: 'Publications',
  components: {
    PublicationModal,
    PublicationViewModal,
    ConfirmModal
  },
  inject: ['showToast'],
  data() {
    return {
      publications: [],
      searchQuery: '',
      typeFilter: '',
      yearFilter: '',
      showModal: false,
      showViewModal: false,
      showDeleteModal: false,
      selectedPublication: null,
      isEditMode: false
    }
  },
  computed: {
    filteredPublications() {
      let filtered = this.publications

      // Search filter
      if (this.searchQuery) {
        const query = this.searchQuery.toLowerCase()
        filtered = filtered.filter(publication => 
          publication.title.toLowerCase().includes(query) ||
          publication.subtitle?.toLowerCase().includes(query) ||
          publication.description.toLowerCase().includes(query) ||
          publication.authors?.some(author => author.toLowerCase().includes(query))
        )
      }

      // Type filter
      if (this.typeFilter) {
        filtered = filtered.filter(publication => publication.type === this.typeFilter)
      }

      // Year filter
      if (this.yearFilter) {
        filtered = filtered.filter(publication => publication.year.toString() === this.yearFilter)
      }

      return filtered
    }
  },
  mounted() {
    this.loadPublications()
    dataManager.initializeData()
  },
  methods: {
    loadPublications() {
      this.publications = dataManager.getPublications()
    },
    
    clearFilters() {
      this.searchQuery = ''
      this.typeFilter = ''
      this.yearFilter = ''
    },
    
    getTypeClass(type) {
      const classes = {
        'Policy Brief': 'bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-300',
        'Manual': 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300',
        'Report': 'bg-purple-100 text-purple-800 dark:bg-purple-900 dark:text-purple-300',
        'Guide': 'bg-orange-100 text-orange-800 dark:bg-orange-900 dark:text-orange-300'
      }
      return classes[type] || 'bg-gray-100 text-gray-800 dark:bg-gray-900 dark:text-gray-300'
    },
    
    openAddModal() {
      this.selectedPublication = null
      this.isEditMode = false
      this.showModal = true
    },
    
    viewPublication(publication) {
      this.selectedPublication = publication
      this.showViewModal = true
    },
    
    editPublication(publication) {
      this.selectedPublication = { ...publication }
      this.isEditMode = true
      this.showModal = true
    },
    
    deletePublication(publication) {
      this.selectedPublication = publication
      this.showDeleteModal = true
    },
    
    downloadPDF(publication) {
      // Simulate PDF download
      this.showToast(`Downloading ${publication.title}...`, 'info')
      // In a real application, this would trigger an actual download
    },
    
    closeModal() {
      this.showModal = false
      this.selectedPublication = null
      this.isEditMode = false
    },
    
    closeViewModal() {
      this.showViewModal = false
      this.selectedPublication = null
    },
    
    closeDeleteModal() {
      this.showDeleteModal = false
      this.selectedPublication = null
    },
    
    savePublication(publicationData) {
      if (this.isEditMode) {
        // Update existing publication
        const index = this.publications.findIndex(p => p.id === publicationData.id)
        if (index !== -1) {
          this.publications[index] = publicationData
          this.showToast('Publication updated successfully!', 'success')
        }
      } else {
        // Add new publication
        const newPublication = {
          ...publicationData,
          id: Math.max(...this.publications.map(p => p.id), 0) + 1,
          downloadCount: 0,
          publishDate: new Date().toISOString().split('T')[0]
        }
        this.publications.push(newPublication)
        this.showToast('Publication added successfully!', 'success')
      }
      
      dataManager.savePublications(this.publications)
      this.closeModal()
    },
    
    confirmDelete() {
      const index = this.publications.findIndex(p => p.id === this.selectedPublication.id)
      if (index !== -1) {
        this.publications.splice(index, 1)
        dataManager.savePublications(this.publications)
        this.showToast('Publication deleted successfully!', 'success')
      }
      this.closeDeleteModal()
    }
  }
}
</script>

<style scoped>
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.line-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>



============================================================
FILE: src/views/Settings.vue
============================================================
<template>
  <div class="space-y-6">
    <!-- Header Section -->
    <div>
      <h1 class="text-2xl font-bold text-gray-900 dark:text-white">Settings</h1>
      <p class="text-gray-600 dark:text-gray-400">Configure your dashboard preferences and manage data</p>
    </div>

    <!-- Appearance Settings -->
    <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
      <div class="flex items-center space-x-3 mb-6">
        <div class="w-10 h-10 bg-purple-100 dark:bg-purple-900 rounded-lg flex items-center justify-center">
          <font-awesome-icon icon="palette" class="text-purple-600 dark:text-purple-400" />
        </div>
        <div>
          <h2 class="text-lg font-semibold text-gray-900 dark:text-white">Appearance</h2>
          <p class="text-sm text-gray-600 dark:text-gray-400">Customize the look and feel of your dashboard</p>
        </div>
      </div>

      <!-- Theme Selection -->
      <div class="space-y-4">
        <h3 class="text-base font-medium text-gray-900 dark:text-white">Theme</h3>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
          <!-- Light Theme -->
          <div 
            @click="setTheme('light')"
            :class="[
              'relative p-4 border-2 rounded-lg cursor-pointer transition-all duration-300',
              currentTheme === 'light' 
                ? 'border-blue-500 bg-blue-50 dark:bg-blue-900/20' 
                : 'border-gray-200 dark:border-gray-600 hover:border-gray-300 dark:hover:border-gray-500'
            ]"
          >
            <div class="flex items-center space-x-3">
              <div class="w-8 h-8 bg-white border border-gray-300 rounded-full flex items-center justify-center">
                <font-awesome-icon icon="sun" class="text-yellow-500" />
              </div>
              <div>
                <h4 class="font-medium text-gray-900 dark:text-white">Light</h4>
                <p class="text-sm text-gray-600 dark:text-gray-400">Clean and bright interface</p>
              </div>
            </div>
            <div v-if="currentTheme === 'light'" class="absolute top-2 right-2">
              <font-awesome-icon icon="check-circle" class="text-blue-500" />
            </div>
          </div>

          <!-- Dark Theme -->
          <div 
            @click="setTheme('dark')"
            :class="[
              'relative p-4 border-2 rounded-lg cursor-pointer transition-all duration-300',
              currentTheme === 'dark' 
                ? 'border-blue-500 bg-blue-50 dark:bg-blue-900/20' 
                : 'border-gray-200 dark:border-gray-600 hover:border-gray-300 dark:hover:border-gray-500'
            ]"
          >
            <div class="flex items-center space-x-3">
              <div class="w-8 h-8 bg-gray-800 border border-gray-600 rounded-full flex items-center justify-center">
                <font-awesome-icon icon="moon" class="text-blue-400" />
              </div>
              <div>
                <h4 class="font-medium text-gray-900 dark:text-white">Dark</h4>
                <p class="text-sm text-gray-600 dark:text-gray-400">Easy on the eyes</p>
              </div>
            </div>
            <div v-if="currentTheme === 'dark'" class="absolute top-2 right-2">
              <font-awesome-icon icon="check-circle" class="text-blue-500" />
            </div>
          </div>

          <!-- System Theme -->
          <div 
            @click="setTheme('system')"
            :class="[
              'relative p-4 border-2 rounded-lg cursor-pointer transition-all duration-300',
              currentTheme === 'system' 
                ? 'border-blue-500 bg-blue-50 dark:bg-blue-900/20' 
                : 'border-gray-200 dark:border-gray-600 hover:border-gray-300 dark:hover:border-gray-500'
            ]"
          >
            <div class="flex items-center space-x-3">
              <div class="w-8 h-8 bg-gradient-to-r from-yellow-400 to-blue-500 rounded-full flex items-center justify-center">
                <font-awesome-icon icon="desktop" class="text-white text-sm" />
              </div>
              <div>
                <h4 class="font-medium text-gray-900 dark:text-white">System</h4>
                <p class="text-sm text-gray-600 dark:text-gray-400">Follow system preference</p>
              </div>
            </div>
            <div v-if="currentTheme === 'system'" class="absolute top-2 right-2">
              <font-awesome-icon icon="check-circle" class="text-blue-500" />
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Notification Settings -->
    <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
      <div class="flex items-center space-x-3 mb-6">
        <div class="w-10 h-10 bg-blue-100 dark:bg-blue-900 rounded-lg flex items-center justify-center">
          <font-awesome-icon icon="bell" class="text-blue-600 dark:text-blue-400" />
        </div>
        <div>
          <h2 class="text-lg font-semibold text-gray-900 dark:text-white">Notifications</h2>
          <p class="text-sm text-gray-600 dark:text-gray-400">Manage your notification preferences</p>
        </div>
      </div>

      <div class="space-y-4">
        <!-- Toast Notifications -->
        <div class="flex items-center justify-between">
          <div>
            <h3 class="text-base font-medium text-gray-900 dark:text-white">Toast Notifications</h3>
            <p class="text-sm text-gray-600 dark:text-gray-400">Show success and error messages</p>
          </div>
          <label class="relative inline-flex items-center cursor-pointer">
            <input 
              v-model="settings.notifications.toast" 
              type="checkbox" 
              class="sr-only peer"
              @change="saveSettings"
            />
            <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"></div>
          </label>
        </div>

        <!-- Sound Notifications -->
        <div class="flex items-center justify-between">
          <div>
            <h3 class="text-base font-medium text-gray-900 dark:text-white">Sound Notifications</h3>
            <p class="text-sm text-gray-600 dark:text-gray-400">Play sounds for important events</p>
          </div>
          <label class="relative inline-flex items-center cursor-pointer">
            <input 
              v-model="settings.notifications.sound" 
              type="checkbox" 
              class="sr-only peer"
              @change="saveSettings"
            />
            <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"></div>
          </label>
        </div>

        <!-- Auto-refresh -->
        <div class="flex items-center justify-between">
          <div>
            <h3 class="text-base font-medium text-gray-900 dark:text-white">Auto-refresh Data</h3>
            <p class="text-sm text-gray-600 dark:text-gray-400">Automatically refresh dashboard data</p>
          </div>
          <label class="relative inline-flex items-center cursor-pointer">
            <input 
              v-model="settings.notifications.autoRefresh" 
              type="checkbox" 
              class="sr-only peer"
              @change="saveSettings"
            />
            <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"></div>
          </label>
        </div>
      </div>
    </div>

    <!-- Data Management -->
    <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
      <div class="flex items-center space-x-3 mb-6">
        <div class="w-10 h-10 bg-red-100 dark:bg-red-900 rounded-lg flex items-center justify-center">
          <font-awesome-icon icon="database" class="text-red-600 dark:text-red-400" />
        </div>
        <div>
          <h2 class="text-lg font-semibold text-gray-900 dark:text-white">Data Management</h2>
          <p class="text-sm text-gray-600 dark:text-gray-400">Manage your dashboard data and storage</p>
        </div>
      </div>

      <div class="space-y-6">
        <!-- Storage Info -->
        <div class="bg-gray-50 dark:bg-gray-700 rounded-lg p-4">
          <h3 class="text-base font-medium text-gray-900 dark:text-white mb-3">Storage Usage</h3>
          <div class="space-y-2">
            <div class="flex justify-between text-sm">
              <span class="text-gray-600 dark:text-gray-400">Projects Data</span>
              <span class="text-gray-900 dark:text-white">{{ storageInfo.projects }} KB</span>
            </div>
            <div class="flex justify-between text-sm">
              <span class="text-gray-600 dark:text-gray-400">Publications Data</span>
              <span class="text-gray-900 dark:text-white">{{ storageInfo.publications }} KB</span>
            </div>
            <div class="flex justify-between text-sm">
              <span class="text-gray-600 dark:text-gray-400">Messages Data</span>
              <span class="text-gray-900 dark:text-white">{{ storageInfo.messages }} KB</span>
            </div>
            <div class="flex justify-between text-sm font-medium border-t border-gray-200 dark:border-gray-600 pt-2">
              <span class="text-gray-900 dark:text-white">Total Usage</span>
              <span class="text-gray-900 dark:text-white">{{ storageInfo.total }} KB</span>
            </div>
          </div>
        </div>

        <!-- Data Actions -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <!-- Export Data -->
          <button 
            @click="exportData"
            class="flex items-center justify-center space-x-3 p-4 border border-green-300 dark:border-green-600 text-green-700 dark:text-green-400 rounded-lg hover:bg-green-50 dark:hover:bg-green-900/20 transition-colors"
          >
            <font-awesome-icon icon="download" />
            <span class="font-medium">Export All Data</span>
          </button>

          <!-- Import Data -->
          <button 
            @click="importData"
            class="flex items-center justify-center space-x-3 p-4 border border-blue-300 dark:border-blue-600 text-blue-700 dark:text-blue-400 rounded-lg hover:bg-blue-50 dark:hover:bg-blue-900/20 transition-colors"
          >
            <font-awesome-icon icon="upload" />
            <span class="font-medium">Import Data</span>
          </button>
        </div>

        <!-- Danger Zone -->
        <div class="border-t border-gray-200 dark:border-gray-700 pt-6">
          <h3 class="text-base font-medium text-red-600 dark:text-red-400 mb-4">Danger Zone</h3>
          <div class="space-y-3">
            <button 
              @click="showResetModal = true"
              class="w-full flex items-center justify-center space-x-3 p-4 bg-red-50 dark:bg-red-900/20 border border-red-300 dark:border-red-600 text-red-700 dark:text-red-400 rounded-lg hover:bg-red-100 dark:hover:bg-red-900/40 transition-colors"
            >
              <font-awesome-icon icon="exclamation-triangle" />
              <span class="font-medium">Reset All Data</span>
            </button>
            <p class="text-xs text-gray-500 dark:text-gray-400 text-center">
              This will permanently delete all projects, publications, and messages. This action cannot be undone.
            </p>
          </div>
        </div>
      </div>
    </div>

    <!-- About Section -->
    <div class="bg-white dark:bg-gray-800 rounded-xl p-6 border border-gray-200 dark:border-gray-700">
      <div class="flex items-center space-x-3 mb-6">
        <div class="w-10 h-10 bg-gray-100 dark:bg-gray-700 rounded-lg flex items-center justify-center">
          <font-awesome-icon icon="info-circle" class="text-gray-600 dark:text-gray-400" />
        </div>
        <div>
          <h2 class="text-lg font-semibold text-gray-900 dark:text-white">About</h2>
          <p class="text-sm text-gray-600 dark:text-gray-400">Dashboard information and version details</p>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div class="space-y-3">
          <div class="flex justify-between">
            <span class="text-gray-600 dark:text-gray-400">Dashboard Version</span>
            <span class="text-gray-900 dark:text-white font-medium">v1.0.0</span>
          </div>
          <div class="flex justify-between">
            <span class="text-gray-600 dark:text-gray-400">Vue.js Version</span>
            <span class="text-gray-900 dark:text-white font-medium">3.5.13</span>
          </div>
          <div class="flex justify-between">
            <span class="text-gray-600 dark:text-gray-400">Last Updated</span>
            <span class="text-gray-900 dark:text-white font-medium">{{ new Date().toLocaleDateString() }}</span>
          </div>
        </div>
        <div class="space-y-3">
          <div class="flex justify-between">
            <span class="text-gray-600 dark:text-gray-400">Organization</span>
            <span class="text-gray-900 dark:text-white font-medium">Sheba Youth Foundation</span>
          </div>
          <div class="flex justify-between">
            <span class="text-gray-600 dark:text-gray-400">Built with</span>
            <span class="text-gray-900 dark:text-white font-medium">Vue.js & Tailwind CSS</span>
          </div>
          <div class="flex justify-between">
            <span class="text-gray-600 dark:text-gray-400">License</span>
            <span class="text-gray-900 dark:text-white font-medium">MIT</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Reset Confirmation Modal -->
    <ConfirmModal
      v-if="showResetModal"
      title="Reset All Data"
      message="Are you sure you want to reset all data? This will permanently delete all projects, publications, and messages. This action cannot be undone."
      @confirm="resetAllData"
      @cancel="showResetModal = false"
    />

    <!-- Hidden file input for import -->
    <input
      ref="fileInput"
      type="file"
      accept=".json"
      @change="handleFileImport"
      class="hidden"
    />
  </div>
</template>

<script>
import { dataManager } from '../data/mockData.js'
import ConfirmModal from '../components/ConfirmModal.vue'

export default {
  name: 'Settings',
  components: {
    ConfirmModal
  },
  inject: ['showToast'],
  data() {
    return {
      currentTheme: 'light',
      showResetModal: false,
      settings: {
        notifications: {
          toast: true,
          sound: false,
          autoRefresh: true
        }
      },
      storageInfo: {
        projects: 0,
        publications: 0,
        messages: 0,
        total: 0
      }
    }
  },
  mounted() {
    this.loadSettings()
    this.calculateStorageUsage()
  },
  methods: {
    loadSettings() {
      // Load theme preference
      const savedTheme = localStorage.getItem('theme') || 'light'
      this.currentTheme = savedTheme
      this.applyTheme(savedTheme)

      // Load other settings
      const savedSettings = localStorage.getItem('dashboard-settings')
      if (savedSettings) {
        this.settings = { ...this.settings, ...JSON.parse(savedSettings) }
      }
    },

    setTheme(theme) {
      this.currentTheme = theme
      localStorage.setItem('theme', theme)
      this.applyTheme(theme)
      this.showToast(`Theme changed to ${theme}`, 'success')
    },

    applyTheme(theme) {
      const html = document.documentElement
      
      if (theme === 'dark') {
        html.classList.add('dark')
      } else if (theme === 'light') {
        html.classList.remove('dark')
      } else if (theme === 'system') {
        // Follow system preference
        const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches
        if (prefersDark) {
          html.classList.add('dark')
        } else {
          html.classList.remove('dark')
        }
      }
    },

    saveSettings() {
      localStorage.setItem('dashboard-settings', JSON.stringify(this.settings))
      this.showToast('Settings saved successfully', 'success')
    },

    calculateStorageUsage() {
      // Calculate approximate storage usage
      const projects = JSON.stringify(dataManager.getProjects()).length / 1024
      const publications = JSON.stringify(dataManager.getPublications()).length / 1024
      const messages = JSON.stringify(dataManager.getMessages()).length / 1024

      this.storageInfo = {
        projects: Math.round(projects * 100) / 100,
        publications: Math.round(publications * 100) / 100,
        messages: Math.round(messages * 100) / 100,
        total: Math.round((projects + publications + messages) * 100) / 100
      }
    },

    exportData() {
      try {
        const data = {
          projects: dataManager.getProjects(),
          publications: dataManager.getPublications(),
          messages: dataManager.getMessages(),
          exportDate: new Date().toISOString(),
          version: '1.0.0'
        }

        const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' })
        const url = URL.createObjectURL(blob)
        const a = document.createElement('a')
        a.href = url
        a.download = `sheba-dashboard-backup-${new Date().toISOString().split('T')[0]}.json`
        document.body.appendChild(a)
        a.click()
        document.body.removeChild(a)
        URL.revokeObjectURL(url)

        this.showToast('Data exported successfully', 'success')
      } catch (error) {
        this.showToast('Failed to export data', 'error')
      }
    },

    importData() {
      this.$refs.fileInput.click()
    },

    handleFileImport(event) {
      const file = event.target.files[0]
      if (!file) return

      const reader = new FileReader()
      reader.onload = (e) => {
        try {
          const data = JSON.parse(e.target.result)
          
          // Validate data structure
          if (!data.projects || !data.publications || !data.messages) {
            throw new Error('Invalid data format')
          }

          // Import data
          dataManager.saveProjects(data.projects)
          dataManager.savePublications(data.publications)
          dataManager.saveMessages(data.messages)

          this.calculateStorageUsage()
          this.showToast('Data imported successfully', 'success')
        } catch (error) {
          this.showToast('Failed to import data. Please check the file format.', 'error')
        }
      }
      reader.readAsText(file)
      
      // Reset file input
      event.target.value = ''
    },

    resetAllData() {
      try {
        // Clear all data
        localStorage.removeItem('dashboard-projects')
        localStorage.removeItem('dashboard-publications')
        localStorage.removeItem('dashboard-messages')
        
        // Reinitialize with default data
        dataManager.initializeData()
        
        this.calculateStorageUsage()
        this.showResetModal = false
        this.showToast('All data has been reset successfully', 'success')
      } catch (error) {
        this.showToast('Failed to reset data', 'error')
      }
    }
  }
}
</script>



============================================================
FILE: src/App.vue
============================================================
<template>
  <div id="app" :class="{ 'dark': isDarkMode, 'light': !isDarkMode }" class="min-h-screen bg-gray-50 dark:bg-gray-900 transition-colors duration-300">
    <!-- Sidebar -->
    <Sidebar 
      :isOpen="sidebarOpen" 
      @toggle="toggleSidebar"
      @close="closeSidebar"
    />
    
    <!-- Main Content -->
    <div :class="{ 'ml-64': sidebarOpen, 'ml-0': !sidebarOpen }" class="transition-all duration-300">
      <!-- Top Navigation -->
      <TopNavigation 
        @toggleSidebar="toggleSidebar"
        @toggleTheme="toggleTheme"
        :isDarkMode="isDarkMode"
      />
      
      <!-- Page Content -->
      <main class="p-6">
        <router-view />
      </main>
    </div>
    
    <!-- Toast Notifications -->
    <Toast ref="toast" />
  </div>
</template>

<script>
import Sidebar from './components/Sidebar.vue'
import TopNavigation from './components/TopNavigation.vue'
import Toast from './components/Toast.vue'

export default {
  name: 'App',
  components: {
    Sidebar,
    TopNavigation,
    Toast
  },
  data() {
    return {
      sidebarOpen: true,
      isDarkMode: false
    }
  },
  mounted() {
    // Load theme preference from localStorage
    const savedTheme = localStorage.getItem('theme')
    if (savedTheme) {
      this.isDarkMode = savedTheme === 'dark'
    } else {
      // Default to system preference
      this.isDarkMode = window.matchMedia('(prefers-color-scheme: dark)').matches
    }
    
    // Load sidebar state from localStorage
    const savedSidebarState = localStorage.getItem('sidebarOpen')
    if (savedSidebarState !== null) {
      this.sidebarOpen = JSON.parse(savedSidebarState)
    }
    
    // Handle responsive sidebar
    this.handleResize()
    window.addEventListener('resize', this.handleResize)
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.handleResize)
  },
  methods: {
    toggleSidebar() {
      this.sidebarOpen = !this.sidebarOpen
      localStorage.setItem('sidebarOpen', JSON.stringify(this.sidebarOpen))
    },
    closeSidebar() {
      this.sidebarOpen = false
      localStorage.setItem('sidebarOpen', JSON.stringify(this.sidebarOpen))
    },
    toggleTheme() {
      this.isDarkMode = !this.isDarkMode
      localStorage.setItem('theme', this.isDarkMode ? 'dark' : 'light')
    },
    handleResize() {
      if (window.innerWidth < 1024) {
        this.sidebarOpen = false
      } else {
        const savedSidebarState = localStorage.getItem('sidebarOpen')
        if (savedSidebarState !== null) {
          this.sidebarOpen = JSON.parse(savedSidebarState)
        } else {
          this.sidebarOpen = true
        }
      }
    },
    showToast(message, type = 'info') {
      this.$refs.toast.show(message, type)
    }
  },
  provide() {
    return {
      showToast: this.showToast
    }
  }
}
</script>



============================================================
FILE: src/main.js
============================================================
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import './style.css'

// FontAwesome
import { library } from '@fortawesome/fontawesome-svg-core'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { 
  faHome, 
  faProjectDiagram, 
  faBook, 
  faEnvelope, 
  faCog, 
  faPlus, 
  faEdit, 
  faTrash, 
  faSearch,
  faBars,
  faTimes,
  faUser,
  faUsers,
  faChartBar,
  faSun,
  faMoon,
  faRefresh,
  faEye,
  faDownload,
  faMapMarkerAlt,
  faCalendar,
  faTag,
  faFileAlt,
  faCheck,
  faExclamationTriangle
} from '@fortawesome/free-solid-svg-icons'

library.add(
  faHome, 
  faProjectDiagram, 
  faBook, 
  faEnvelope, 
  faCog, 
  faPlus, 
  faEdit, 
  faTrash, 
  faSearch,
  faBars,
  faTimes,
  faUser,
  faUsers,
  faChartBar,
  faSun,
  faMoon,
  faRefresh,
  faEye,
  faDownload,
  faMapMarkerAlt,
  faCalendar,
  faTag,
  faFileAlt,
  faCheck,
  faExclamationTriangle
)

const app = createApp(App)

app.component('font-awesome-icon', FontAwesomeIcon)
app.use(router)

app.mount('#app')



============================================================
FILE: src/style.css
============================================================
@import 'tailwindcss';

/* Custom CSS Variables for Sheba Theme */
:root {
  --primary-cyan: #00BCD4;
  --primary-orange: #FF5722;
  --primary-purple: #9C27B0;
  --primary-pink: #E91E63;
  --secondary-blue: #2196F3;
  --secondary-green: #4CAF50;
  --text-dark: #2C3E50;
  --text-light: #FFFFFF;
  --bg-light: #F8F9FA;
  --bg-dark: #1A1A1A;
  --border-light: #E0E0E0;
  --border-dark: #333333;
}

/* Dark mode styles */
.dark {
  --bg-primary: var(--bg-dark);
  --text-primary: var(--text-light);
  --border-primary: var(--border-dark);
}

.light {
  --bg-primary: var(--bg-light);
  --text-primary: var(--text-light);
  --border-primary: var(--border-light);
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 6px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: var(--primary-cyan);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: var(--primary-orange);
}

/* Smooth transitions */
* {
  transition: all 0.3s ease;
}

/* Custom gradient backgrounds */
.gradient-primary {
  background: linear-gradient(135deg, var(--primary-cyan) 0%, var(--primary-orange) 100%);
}

.gradient-secondary {
  background: linear-gradient(135deg, var(--primary-purple) 0%, var(--primary-pink) 100%);
}

/* Animation classes */
.fade-in {
  animation: fadeIn 0.5s ease-in;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.slide-in-left {
  animation: slideInLeft 0.3s ease-out;
}

@keyframes slideInLeft {
  from { transform: translateX(-100%); }
  to { transform: translateX(0); }
}

/* Toast notification styles */
.toast {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 1000;
  min-width: 300px;
  padding: 16px;
  border-radius: 8px;
  color: white;
  font-weight: 500;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  animation: slideInRight 0.3s ease-out;
}

@keyframes slideInRight {
  from { transform: translateX(100%); }
  to { transform: translateX(0); }
}

.toast.success {
  background: var(--secondary-green);
}

.toast.error {
  background: #f44336;
}

.toast.warning {
  background: #ff9800;
}

.toast.info {
  background: var(--secondary-blue);
}



============================================================
FILE: jsconfig.json
============================================================
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "exclude": ["node_modules", "dist"]
}



============================================================
FILE: package.json
============================================================
{
  "name": "sheba-admin-dashboard",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "engines": {
    "node": "^20.19.0 || >=22.12.0"
  },
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "@fortawesome/fontawesome-svg-core": "^7.0.0",
    "@fortawesome/free-brands-svg-icons": "^7.0.0",
    "@fortawesome/free-solid-svg-icons": "^7.0.0",
    "@fortawesome/vue-fontawesome": "^3.1.1",
    "@tailwindcss/vite": "^4.1.11",
    "vue": "^3.5.18",
    "vue-router": "^4.5.1"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^6.0.1",
    "vite": "^7.0.6",
    "vite-plugin-vue-devtools": "^8.0.0"
  }
}



============================================================
FILE: README.md
============================================================
# vue-project

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```


============================================================
FILE: vite.config.js
============================================================
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import tailwindcss from '@tailwindcss/vite'
import vueDevTools from 'vite-plugin-vue-devtools'

// https://vite.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    vueDevTools(),
    tailwindcss(),

  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    },
  },
})