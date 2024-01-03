<script lang="ts" setup>
import { ref, watch } from "vue";
import { Select } from "@element-plus/icons-vue";

interface ColorItme {
	value?: string;
	code?: string;
	style?: any;
}

const props = withDefaults(
	defineProps<{
		disabled?: boolean;
		color?: "#3E71F0" | "#FECE02" | "#27C998" | "#0FBC86";
		modelValue: string;
	}>(),
	{
		disabled: false,
		color: "#0FBC86",
		modelValue: ""
	}
);

const emits = defineEmits<{
	(e: "update:modelValue", val: string): void;
	(e: "update:color", val: string): void;
}>();

const currColor = ref("");
const inputRef = ref();
const carNo = ref("");
const area = ref([
	"京",
	"津",
	"沪",
	"渝",
	"冀",
	"豫",
	"云",
	"辽",
	"黑",
	"湘",
	"皖",
	"鲁",
	"新",
	"苏",
	"浙",
	"赣",
	"鄂",
	"桂",
	"甘",
	"晋",
	"蒙",
	"陕",
	"吉",
	"贵",
	"粤",
	"青",
	"藏",
	"川",
	"宁",
	"琼"
]);
const areaLetter = ref([
	"A",
	"B",
	"C",
	"D",
	"E",
	"F",
	"G",
	"H",
	"J",
	"K",
	"L",
	"M",
	"N",
	"O",
	"P",
	"Q",
	"R",
	"S",
	"T",
	"U",
	"V",
	"W",
	"X",
	"Y",
	"Z",
	"挂"
]);
const areaNum = ref(["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"]);
const colors = ref<ColorItme[]>([
	{
		value: "#3E71F0",
		code: "豫A·88888",
		style: {
			backgroundColor: "#3E71F0",
			color: "#fff"
		}
	},
	{
		value: "#FECE02",
		code: "豫A·F8888",
		style: {
			backgroundColor: "#FECE02",
			color: "#000"
		}
	},
	{
		value: "#27C998",
		code: "豫A·88888F",
		style: {
			background: "linear-gradient(to right, #FECE02 38%, #27C998 38% 40%, #27C998 40% 100%)",
			color: "#000"
		}
	},
	{
		value: "#0FBC86",
		code: "豫A·D88888",
		style: {
			backgroundColor: "#0FBC86",
			color: "#fff"
		}
	}
]);

watch(
	() => props.modelValue,
	() => {
		carNo.value = props.modelValue;
	},
	{
		immediate: true
	}
);

watch(
	() => carNo.value,
	() => {
		emits("update:modelValue", carNo.value);
	}
);

watch(
	() => props.color,
	() => {
		currColor.value = props.color;
	}
);

const selectCarNo = (item: string) => {
	carNo.value = carNo.value + item;
	inputRef.value && inputRef.value.focus();
};

const colorClick = (item: ColorItme) => {
	currColor.value = item.value as string;
	emits("update:color", currColor.value);
};
</script>

<template>
	<el-popover placement="bottom" popper-class="car-no-popover" :width="360" trigger="click" :disabled="disabled">
		<template #reference>
			<el-input ref="inputRef" clearable v-model="carNo" :disabled="disabled" />
		</template>
		<div class="choose-list">
			<div class="list-row">
				<div class="list-item" v-for="item in area" :key="item">
					<button @click="selectCarNo(item)">{{ item }}</button>
				</div>
			</div>
			<div class="list-row">
				<div class="list-item" v-for="item in areaLetter" :key="item">
					<button @click="selectCarNo(item)">{{ item }}</button>
				</div>
			</div>
			<div class="list-row">
				<div class="list-item" v-for="item in areaNum" :key="item">
					<button @click="selectCarNo(item)">{{ item }}</button>
				</div>
			</div>
			<div class="color-row list-row">
				<div class="list-label">车牌颜色:</div>
				<div class="list-item">
					<button
						v-for="item in colors"
						:key="item.value"
						@click="colorClick(item)"
						:class="item.value == color ? 'btn-checked' : ''"
						style="width: 40%"
						:style="item.style"
					>
						{{ item.code }}
						<span>
							<el-icon><Select /></el-icon>
						</span>
					</button>
				</div>
			</div>
		</div>
	</el-popover>
</template>

<style lang="scss" scoped>
.car-no-popover {
	.choose-list {
		display: flex;
		flex-direction: column;
	}
	.list-row {
		display: flex;
		flex-wrap: wrap;
		&:not(:first-child) {
			margin-top: 10px;
		}
		&:last-child {
			margin-top: 0px;
		}
	}
	.list-item {
		margin-top: 6px;
		margin-left: 6px;
		button {
			width: 26px;
			height: 23px;
			line-height: 20px;
			color: #333333;
			cursor: pointer;
			background-color: #ffffff;
			border: 1px solid #cccccc;
			border-radius: 3px;
			transition: all 0.15s linear;
			&:hover {
				font-weight: bold;
				color: #222222;
				background-color: #cccccc;
			}
		}
	}
	.color-row {
		align-items: center;
		.list-label {
			width: 20%;
		}
		.list-item {
			width: 77%;
			button {
				width: 50%;
				margin-left: 8%;
				margin-top: 3%;
				position: relative;
				span {
					position: absolute;
					top: -3px;
					right: -7px;
					font-weight: bold;
					font-size: 14px;
					color: #111;
					display: none;
				}
			}
			.btn-checked {
				span {
					display: inline-block;
				}
			}
		}
	}
}
</style>
