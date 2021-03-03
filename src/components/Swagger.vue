<template>
  <div :id="id" :class="classObject">
    <Loading />
    <slot />
  </div>
</template>

<script>
import SwaggerUIBundle from "swagger-ui";
import Vue from "vue";

import { get } from "axios";
import "swagger-ui/dist/swagger-ui.css";

import CopyUrlBase from "./CopyUrlBase";
import Loading from "./Loading";

export default {
  props: {
    noModel: {
      type: Boolean,
      default: false,
    },
    noTitle: {
      type: Boolean,
      default: false,
    },
    urlBase: {
      type: String,
      required: true,
    },
    url: {
      type: String,
      required: true,
    },
  },
  components: {
    Loading,
  },
  computed: {
    classObject() {
      return {
        "swagger--no-models": this.noModel,
        "swagger--no-title": this.noTitle,
        swagger: true,
      };
    },
  },
  data() {
    return {
      id: null,
    };
  },
  async mounted() {
    //todo: separete responsabilities
    this.id = `swagger-${this._uid}`;

    const result = (await get(this.url)).data;

    const tags = this.$children.map((child) => child?.$props?.tag ?? "");

    const operationIds = this.$children.map(
      (child) => child?.$props?.operationId ?? ""
    );

    const filteredTags = [];

    for (let pathName in result.paths) {
      const methods = result.paths[pathName];
      for (let methodName in methods) {
        const method = result.paths[pathName][methodName];
        const containsTag = method.tags.filter((tag) =>
          tags.find((_tag) => _tag.toUpperCase() == tag.toUpperCase())
        ).length;
        const containsOperationId = operationIds.find(
          (op) => op == method.operationId
        );
        if (!containsTag && !containsOperationId)
          delete result.paths[pathName][methodName];
        else filteredTags.concat(result.paths[pathName][methodName].tags);
      }
      const hasAnyProperty = Object.getOwnPropertyNames(result.paths[pathName])
        .length;
      if (!hasAnyProperty) delete result.paths[pathName];
    }
  
    result.tags =
      result.tags?.filter((tag) =>
        filteredTags.find((filteredTag) => tag.name == filteredTag)
      ) ?? [];

    SwaggerUIBundle({
      dom_id: `#${this.id}`,
      requestInterceptor(request) {
        request.url = request.url.replace(window.location.origin, this.urlBase);
        request.headers["Authorization"] = `Bearer  ${window["JSM_DEV_TOKEN"]}`;
        return request;
      },
      spec: result,
      docExpansion: "none",
      presets: [
        SwaggerUIBundle.presets.apis,
        SwaggerUIBundle.SwaggerUIStandalonePreset,
      ],
    });

    let copyUrlBase = new Vue({
      ...CopyUrlBase,
      parent: this,
      propsData: {
        urlBase: this.urlBase,
      },
    }).$mount();

    document.querySelector(`#${this.id} .description`).after(copyUrlBase.$el);
  },
};
</script> 
<style >
.swagger--no-models .models {
  display: none !important;
}

.swagger--no-title .information-container .title {
  display: none !important;
}

.base-url,
.scheme-container {
  display: none !important;
}
</style>