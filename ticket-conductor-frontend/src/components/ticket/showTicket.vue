<template>
	<v-layout row wrap>
		<v-flex lg8>
			<v-card light>
				<v-card-title  primary-title class="title pb-0">{{ ticket.title }}</v-card-title>
				<v-subheader class="caption">{{ consumer.first_name }} {{ consumer.last_name }}
				<router-link :to="{name: 'showConsumer', params: {id: consumer.id}}"> {{ consumer.email }} </router-link></v-subheader>
				<v-card-text>{{ticket.description}}</v-card-text>
			</v-card>
			<create-comment class="mt-4 mb-4"></create-comment>
			<comments :ticketId="ticketId"></comments>
		</v-flex>
		<v-flex lg4>
			<v-card>
				<v-card-text>
					<sidebar  v-if="ticket" :ticket="ticket"></sidebar>
				</v-card-text>
			</v-card>
		</v-flex>
	</v-layout>
</template>

<script>
	import axios from 'axios'
	import newTicketModal from './createTicketModal.vue'
	import moment from 'moment'
	import config from '../../../config/config.js'
	import auth from '../../auth/index.js'
	import ticketSidebar from './ticketSidebar.vue'
	import createComment from '../comment/createComment.vue'
	import comment from '../comment/comment.vue'

	export default {
		data () {
			return {
				ticket: "",
				consumer: {}
			}
		},
		methods: {
			getTicket() {
				var self = this
				axios.get(config.api_url + '/ticket/' + this.$route.params.id , {
					headers: auth.getAuthHeader()
				})
				.then(function (response) {
					self.ticket = response.data

				})
				.catch(function (error) {

				});

			},
			getConsumer() {
				var self = this
				axios.get(config.api_url + '/ticket/' + this.$route.params.id + '/consumer' , {
					headers: auth.getAuthHeader()
				})
				.then(function (response) {
					self.consumer = response.data
				})
				.catch(function (error) {

				});

			}
		},
		computed: {
			ticketId () {
				return this.$route.params.id
			}
		},
		mounted: function () {
			this.getTicket()
			this.getConsumer()
		},
		components: {
			'new-ticket-modal': newTicketModal,
			'sidebar': ticketSidebar,
			'create-comment': createComment,
			'comments': comment
		},
		filters: {
			formatDate: function(date) {
				return moment(date).fromNow()
			}
		}
	}
</script>
<style>

</style>
