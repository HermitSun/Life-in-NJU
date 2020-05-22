<template>
  <v-row justify="center" align="center" class="ml-4 mr-4">
    <v-expansion-panels multiple>
      <v-expansion-panel v-for="(category, i) in data" :key="i" class="panels">
        <v-expansion-panel-header>
          {{ category.title }}
        </v-expansion-panel-header>
        <v-expansion-panel-content>
          <v-row>
            <v-col
              class="pa-1 d-flex flex-column"
              cols="12"
              lg="3"
              v-for="(item, i) in category.items"
              :key="i"
            >
              <v-card
                outlined
                tile
                @click="item.link === '' ? null : open(item.link)"
                class="panel flex d-flex flex-column"
                :title="item.description"
              >
                <v-card-text>
                  <p class="ma-0 font-weight-black">
                    {{ item.name }}
                  </p>
                  <p class="ma-0 flex">
                    {{ item.description }}
                  </p>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
  </v-row>
</template>

<script>
import data from "@/assets/data.json";

export default {
  name: "Menu",
  data() {
    return {
      data
    };
  }
};
</script>

<style scoped src="@/assets/style/menu.css"></style>
