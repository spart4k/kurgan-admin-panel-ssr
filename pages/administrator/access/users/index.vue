<template>
	<div class="main">
		<Topbar
		:title="'Пользователи'"
		:history="false"
		>
			<div slot="history" class="">
				<NuxtLink to="/index">
					
				</NuxtLink>
			
			</div>
		</Topbar>
		<div 
		:class="{ loading: globalLoading }"
		class="main-content full">
			<!-- <TableServices 
			:data="getAllServices.rows"
			:headers="headers"
			></TableServices> -->
			
			<div  
			v-if="!globalLoading"
			class="table">
				<div class="table-header">
					<div class="table-header-panel">
						<div class="input withicon">
							<input 
							v-model="searchInput"
							placeholder="Найти" 
							class="input table-header-panel__search" type="text">
							<div 
							@click="clearSearch"
							>
								<IconClose
								v-if="searching"
								></IconClose>
							</div>
							
						</div>
						

						<div
						@click="searchTable"
						class="table-header-panel__btn btn black">
							Поиск
						</div>
					</div>
					
				</div>

				<table >
					<thead>
					  	<tr>
					   		<td 
					   		v-for="(item,key,index) in headers"
					   		> 
					   			{{ item.title }}
					   		</td>
					   		
					  	</tr>
					</thead>

					<tbody>
					 	<tr 
					 	v-for="(item,key,index) in responseData.rows"
					 	>
					 		<td>
					 			{{ item.email }}
					 		</td>
							 <td>
					 			{{ item.name }}
					 		</td>
					 		
					 		
					 		<td>
					 			Телефон 
					 		</td>
							 
							<td>
								 <div 
								 :class="getUserStatusStyle(item.blocked)"
								 class="status">
									 {{ getUserStatus(item.blocked) }} 
								 </div>
					 			
					 		</td>
					 		

					 		<td >
					 			<TableSettings

					 			:type="'user'"
					 			:status="item.status"
					 			:item="item"
					 			@checkUser="checkUser(item)"
								@changeUserStatus="changeUserStatus(item)"
					 			>
					 				
					 			</TableSettings>

					 		</td>
					 	</tr>
					 </tbody>
				</table>

				<div class="table-footer">

					<TablePagination
					:responseData="responseData"
					:currentPage="currentPage"
					@sortListInPaginate="sortListInPaginate"
					@paginationNext="paginationNext"
					@paginationPrev="paginationPrev"
					@getQtyPage="getQtyPage"
					@paginationStart="paginationStart"
					@paginationEnd="paginationEnd"
					></TablePagination>

					
				</div>

			</div>
				<Loader
				v-if="globalLoading"
				></Loader>

				<ViewingItem
				
				@previewHide="previewHide"
				:card="cardInfo"
				:type="'user'"
				:show="previewShowing"
				@changeUserStatus="changeUserStatus"
				>
				
				</ViewingItem>
				
				
			
		</div>
	</div>
</template>

<script>
	import { mapGetters, mapActions } from 'vuex'
	import tableMixin from '@/mixins/table';
	import paginationMixin from '@/mixins/pagination';

	export default {
		mixins: [
			tableMixin,
			paginationMixin
		],
		data() {
			return {
				headers: [
					{
						title: 'E-mail',
						name: 'E-mail',
						sort: false,
					},
					{
						title: 'Имя',
						name: 'serivce',
						sort: false,
					},
					{
						title: 'Телефон',
						name: 'status',
						sort: false,
					},
					{
						title: 'Статус',
						name: 'status',
						sort: false,
					},
					{
						title: '',
						name: 'settings',
						sort: false,
					}
				],
				cardInfo: [],
				forQuery: {
					role: 'USER',
					offset: 0,
					limit: 20
				},
			}
		},
		layout: 'admin',
		computed: {	
			...mapGetters({
				getUsers: 'admin/users/getUsers',
			}),
			getParamsForQuery() { 
				return `admin/users?role=${this.forQuery.role}&offset=${this.forQuery.offset}&limit=${this.forQuery.limit}&search=${this.searchInput}`
			}
		},
		methods: {
			...mapActions({
				queryData: 'service/getData'
			}),

			checkUser(item) {
				this.previewShow()
				this.cardInfo = item
				console.log(item)
			},

		},
		mounted() {
			this.getData()
		}
	}
</script>