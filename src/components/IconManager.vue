<script setup lang="ts">
import _ from "lodash";
import * as HeroIconsOutline from "@heroicons/vue/24/outline";
import * as HeroIconsSolid from "@heroicons/vue/24/solid";
import * as LocalIcons from "./icons";
import { h, PropType, RendererElement, RendererNode, toRefs, VNode } from "vue";

const definedProps = defineProps({
  name: {
    type: String,
    required: true,
  },
  brand: {
    type: String as PropType<"heroicons" | "local">,
    default: "local",
  },
  mode: {
    type: String as PropType<"outline" | "solid">,
    default: "outline",
  },
});
const props = toRefs(definedProps);

const iconName = _.upperFirst(_.camelCase(props.name.value));
const brandName = props.brand.value;
const iconMode = _.lowerCase(props.mode.value);

let Icon: VNode<RendererNode, RendererElement>;
if (brandName === "heroicons" && iconMode === "outline") {
  const tmp = _.get(HeroIconsOutline, `${iconName}Icon`);
  if (tmp !== undefined) {
    Icon = tmp as VNode<RendererNode, RendererElement>;
  }
} else if (brandName === "heroicons" && iconMode === "solid") {
  const tmp = _.get(HeroIconsSolid, `${iconName}Icon`);
  if (tmp !== undefined) {
    Icon = tmp as VNode<RendererNode, RendererElement>;
  }
} else if (brandName === "local") {
  const tmp = _.get(LocalIcons, `${iconName}Icon`);
  if (tmp !== undefined) {
    Icon = tmp as VNode<RendererNode, RendererElement>;
  }
} else {
  Icon = h("span", "Error loading icon");
}
</script>

<template>
  <component :is="Icon" />
</template>
