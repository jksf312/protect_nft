<template>
	<div class="admin">
		<!-- <left-nav /> -->
		<!-- Dashboard -->
		<v-main class="dashboard pl-6">
			<v-container>
				<!-- Content -->
				<v-row wrap class="dashboard-content">
					<v-col>
						<v-btn @click="kaikasCall">kaikas 호출</v-btn>
						<v-btn @click="tokenAdd">kaikas token add</v-btn>
						<!-- <app-main /> -->
					</v-col>
				</v-row>
			</v-container>
		</v-main>
	</div>
</template>
<script>
// import { AppMain } from '../codeMachine'
import Caver from 'caver-js'
export default {
	components: {
		// AppMain,
		// leftNav,
	},
	methods: {
		kaikasCall() {
			if (window.klaytn) {
				// kaikas와 상호작용해서 모든 공개키 획득
				// accounts[0] 같이 배열로 접근하여 사용가능
				window.klaytn.enable()

				// 현재 kaikas에 선택된 공개키
				const account = window.klaytn.selectedAddress
				// console.log(account)

				// caver-js 연결
				const caver = new Caver(window.klaytn)
				caver.klay.getBalance(account).then(d => console.log(d))
			}
		},
		tokenAdd() {
			// caver 함수 중 현재 공개키의 klay양을 리턴하는 함수
			const tokenAddress = '0xd00981105e61274c8a5cd5a88fe7e037d935b513'
			const tokenSymbol = 'TUT'
			const tokenDecimals = 18
			const tokenImage = 'http://placekitten.com/200/300'

			window.klaytn.sendAsync(
				{
					method: 'wallet_watchAsset',
					params: {
						type: 'ERC20', // Initially only supports ERC20, but eventually more!
						options: {
							address: tokenAddress, // The address that the token is at.
							symbol: tokenSymbol, // A ticker symbol or shorthand, up to 5 chars.
							decimals: tokenDecimals, // The number of decimals in the token
							image: tokenImage, // A string url of the token logo
						},
					},
					id: Math.round(Math.random() * 100000),
				},
				(err, added) => {
					if (added) {
						console.log('Thanks for your interest!')
					} else {
						console.log('Your loss!')
					}
				},
			)
		},
	},
}
</script>
<style lang="scss" scoped>
.dashboard {
	max-width: 87% !important;
}
</style>
