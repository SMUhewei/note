# vue3

2023.10.14

响应式数据(ref, reactive, computed, watch)

## vue3核心语法

### ref创建：基本类型的响应式数据

ref ====> 基本类型数据

### reactive创建：对象类型的响应式数据

reactive====>对象类型数据

```html
<temple>
	<div>
        <h2>
            汽车信息：一辆{{car.brand}}车，价值{{car.price}}万
        </h2>
        <button @click="changePrice">
            修改汽车价格
        </button>
    </div>
</temple>
<script lang="ts" setup name="Car">
	import {reactive} from 'vue'
    
    //数据
    let car = reactive({brand:'奔驰', price:100})
    
    //方法
    function changePrice(){
        car.price += 10
        console.log(car.price)
    }
</script>

```

### toRefs和toRef

### computed

```html
<template>
    <div class="person">
    	姓：<input type="text"> <br>
        名：<input type="text"> <br>
        全名：<span>{{fullName}} </span> <br>
    </div>
</template>
<scritp lang="ts" setup name="Person">
    import {ref} from "vue"
	let firstName = ref("张")
    let lastName = ref("三")
    //computed有缓存，方法没有缓存
    //这么定义的fullName是一个computed，且是只读的
    //let fullName = computed(() => {
    //	return firstName.value.slice(0,1).toUpperCase + firstName.value.slice(1) + "-" + lastName.value
    //})
    
    //这么定义fullName是一个计算属性，且是可读可写的
    let fullName = computed({
    	get(){
    		return firstName.value.slice(0,1).toUpperCase + firstName.value.slice(1) + "-" + lastName.value
    	},
    	set(val){
    		const [str1, str2] = val.split('-')
    		firstName.value = str1
    		lastName.value = str2
    	}
    }
    function changeFullName(){
    	funllName.value = "li-si"
    }
</scritp>
<style scoped>
	
</style>
```

