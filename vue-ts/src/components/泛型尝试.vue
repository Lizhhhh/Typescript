<template>
  <div>

  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

// Feature 接口
interface Feature {
  id: number;
  name: string;
  version: string;
}

// 声明泛型
interface Result<T> {
  ok: 0 | 1;
  data: T[];
}

// 声明泛型函数
function getData<T>(): Result<T> {
  const data: any[] = [
    { id: 1, name: "类型注解", version: "2.0" },
    { id: 2, name: "编译型语言", version: "1.0" }
  ];
  return { ok: 1, data } as Result<T>;
}

@Component
export default class Generics extends Vue {
  // 初始化一个接口类型的feature
  private feature: Feature[] = [];

  created() {
    this.feature = getData<Feature>().data;
  }
}
</script>

<style scoped>
</style>