<template>
  <section class="sidebar">
    <select class="sidebar__filter" name="parts" @change="$emit('selectChange', activeF)" :value="activeF" v-model="activeF">
        <option value="all" slected>all</option>
        <option v-for="(item, i) in filterOptions" :key="i" :value="item.value">{{ item.label }}</option>
    </select>

    <table>
        <tr>
            <th v-for="(item, i) in tableHead" :key="i">{{ item }}</th>
        </tr>
        <tr v-for="item, i in computedList" :key="i">
            <td>
                <!-- <img :src="item.imageUrl" alt="" width="60" height="60"> -->
                {{ item.label }}
            </td>
            <td>{{ item.maker }}</td>
            <td>{{ item.description }}</td>
            <td>{{ item.price }}</td>
        </tr>
    </table>

  </section>
</template>

<script setup>
import { computed, defineProps, ref, toRef, watch } from 'vue';

defineEmits(['selectChange']);
const props = defineProps({
    db: {
        type: Object,
        required: true
    },
    activeFilter: {
        type: String,
        required: true,
        default: 'all'
    },
    filterOptions: {
        type: Array,
        required: true,
    },
    tableHead: {
        type: Array,
        required: true
    },
});

const activeF = ref('all');

watch(() => props.activeFilter, (newValue, oldValue) => {
    console.log(newValue, oldValue);
    activeF.value = newValue;
}, { immediate: true });

const flattenObj = (obj, parent, res = {}) => {
    for(let key in obj) {
        let propName = parent ? parent + '_' + key : key;
        if (typeof obj[key] === typeof {} && !Array.isArray(obj[key])) {
            flattenObj(obj[key], propName, res);
        } else {
            res[propName] = obj[key];
        }
    }
    return res;
}
const listAll = flattenObj(props.db);

const computedList = computed({
    get: () => {
        if (activeF.value === 'all') {
            return Object.values(listAll).flat();
        }

        return listAll[`front_${activeF.value}`];
    }
})
</script>

<style scoped>
.sidebar {
    width: 33vw;
    display: flex;
    flex-direction: column;
    padding: 24px;
    border-right: 1px solid #9e9e9e;

    &__filter {
        width: max-content;
    }
}
</style>
