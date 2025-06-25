<script setup>
import { ref, computed } from "vue"
import { leftItems, rightItems } from "./constants"

const selectedLeftItems = ref([])
const selectedRightItem = ref(null)

const isLeftItemDisabled = computed(() => selectedLeftItems.value.length >= 6)

const activeLeftItems = computed(() => {
	return leftItems.filter(({ id }) => !selectedLeftItems.value.includes(id))
})

const activeRightItems = computed(() => {
	return rightItems.filter(item => item.id !== selectedRightItem.value)
})

function selectItem(listType, id) {
	if (listType === "left") {
		selectedLeftItems.value.push(id)
	} else {
		selectedRightItem.value = id
	}
}

function removeItem(listType, id) {
	if (listType === "left") {
		selectedLeftItems.value = selectedLeftItems.value.filter(
			itemId => itemId !== id
		)
	} else {
		selectedRightItem.value = null
	}
}

function displayItemName(arr, id) {
	return arr.find(item => item.id === id)?.name || ""
}
</script>

<template>
	<div class="select-area">
		<div class="select__list select__top-left">
			<div
				v-for="id in selectedLeftItems"
				:key="id"
				@click="removeItem('left', id)"
				class="item"
			>
				<button class="item__name">{{ displayItemName(leftItems, id) }}</button>
			</div>
		</div>
		<div class="select__list select__top-right">
			<div
				v-if="selectedRightItem"
				@click="removeItem('right', selectedRightItem)"
				class="item"
			>
				<button class="item__name">
					{{ displayItemName(rightItems, selectedRightItem) }}
				</button>
			</div>
		</div>

		<div class="select__list select__bottom-left">
			<div
				v-for="item in activeLeftItems"
				:key="item.id"
				@click="selectItem('left', item.id)"
				class="item"
			>
				<button class="item__name" :disabled="isLeftItemDisabled">
					{{ item.name }}
				</button>
			</div>
		</div>

		<div class="select__list select__bottom-right">
			<div
				v-for="item in activeRightItems"
				:key="item.id"
				@click="selectItem('right', item.id)"
				class="item"
			>
				<button class="item__name">{{ item.name }}</button>
			</div>
		</div>
	</div>
</template>

<style scoped>
.select-area {
	display: grid;
	grid-template-columns: 1fr 1fr;
	column-gap: 16px;
	row-gap: 8px;
}

.select__list {
	display: flex;
	column-gap: 8px;
	padding: 16px;
	border: 1px solid black;
	min-height: 30px;
}

.item__name {
	height: 30px;
	user-select: none;
}
</style>
