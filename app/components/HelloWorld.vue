<template>
  <Page>
    <ActionBar title="Shopping App" />

    <StackLayout>
      <BottomNavigation>
        <TabStrip>
          <TabStripItem>
            <Label text="Ladies"></Label>
            <Image src="res://home"></Image>
          </TabStripItem>
          <TabStripItem>
            <Label text="Gents"></Label>
            <Image src="res://search"></Image>
          </TabStripItem>
          <TabStripItem>
            <Label text="My Cart"></Label>
            <Image src="res://favorites"></Image>
          </TabStripItem>
          <TabStripItem>
            <Label text="Figures"></Label>
            <Image src="res://browse"></Image>
          </TabStripItem>
        </TabStrip>
        <!-- ladies -->
        <TabContentItem>
          <ListView for="product in ladies" @itemTap="onItemTap">
            <v-template>
              <StackLayout orientation="vertical" height="300">
                <Image :src="product.image" stretch="aspectFill" />
                <Label :text="product.name" class="h2" />
              </StackLayout>
            </v-template>
          </ListView>
        </TabContentItem>
        <!-- gents -->
        <TabContentItem>
          <ListView for="product in gents" @itemTap="onItemTap">
            <v-template>
              <StackLayout orientation="vertical" height="300">
                <Image :src="product.image" stretch="aspectFill" />
                <Label :text="product.name" class="h2" />
              </StackLayout>
            </v-template>
          </ListView>
        </TabContentItem>
        <!-- my cart -->
        <TabContentItem>
          <StackLayout orientation="vertical" margin="10">
            <Label text="My Shopping Cart" class="h2" />
            <ListView for="product in inCart" @itemTap="onItemTap" height="300">
              <v-template>
                <FlexboxLayout flexDirection="row">
                  <Image :src="product.image" class="thumb img-circle" />
                  <Label
                    :text="product.name + ' x ' + product.quantity"
                    class="t-12"
                    style="width: 60%"
                  />
                </FlexboxLayout>
              </v-template>
            </ListView>
            <Label :text="'Total: $' + total" class="h2" />
          </StackLayout>
        </TabContentItem>
        <!-- figures -->
        <TabContentItem>
          <GridLayout rows="*" height="1000px">
            <RadCartesianChart row="0" style="font-size: 12;">
              <BarSeries
                v-tkCartesianSeries
                :items="chartData"
                categoryProperty="name"
                valueProperty="quantity"
              />
              <CategoricalAxis v-tkCartesianHorizontalAxis />
              <LinearAxis v-tkCartesianVerticalAxis />
            </RadCartesianChart>
          </GridLayout>
        </TabContentItem>
      </BottomNavigation>
    </StackLayout>
  </Page>
</template>

<script>
import Vue from "nativescript-vue";
import RadCartesianChart from "nativescript-ui-chart/vue";
import ProductDetail from "./ProductDetail";
Vue.use(RadCartesianChart);

export default {
  mounted() {
    console.log("in mounted ");
    this.gents = this.products.filter(function (p) {
      return p.department == "Gents";
    });
    this.ladies = this.products.filter(function (p) {
      return p.department == "Ladies";
    });
  },

  methods: {
    onItemTap: function (args) {
      console.log("Item with index: " + args.index + " tapped");
      console.log("Product selected: " + args.item.name);

      this.$navigateTo(ProductDetail, {
        transition: {},
        transitionIOS: {},
        transitionAndroid: {},
        props: {
          selectedProduct: args.item,
          $delegate: this,
        },
      });
    },

    updateCart: function () {
      var sum = 0;
      this.inCart = this.products.filter(function (p) {
        sum += p.quantity * p.price;
        return p.quantity > 0;
      });
      this.total = sum;
      this.chartData = this.products.slice();
    },
  },

  data() {
    return {
      chartData: [],
      inCart: [],
      total: 0,
      ladies: [],
      gents: [],
      products: [
        {
          id: "1",
          department: "Gents",
          name: "Denim Men's Shirts",
          desc: "Passion, dedication, hard work and creativity",
          image: "https://cdn.stocksnap.io/img-thumbs/960w/GAVVVYCGXC.jpg",
          price: 450,
          quantity: 0,
        },
        {
          id: "2",
          department: "Ladies",
          name: "RUN Hoodies",
          desc: "Top-quality, instant-favorite sweatshirt",
          image: "https://cdn.stocksnap.io/img-thumbs/960w/UHAQDIYT6X.jpg",
          price: 600,
          quantity: 0,
        },
        {
          id: "3",
          department: "Gents",
          name: "Men's Lightweight Coat",
          desc: "Signature Buck appliqué, and triple needle stitching ",
          image: "https://cdn.stocksnap.io/img-thumbs/960w/PJY3Y7010M.jpg",
          price: 2500,
          quantity: 0,
        },
        {
          id: "4",
          department: "Ladies",
          name: "Story Story Tee",
          desc:
            "This is an ultra soft T-shirt with a stylish loose and extra long fit ",
          image: "https://cdn.stocksnap.io/img-thumbs/960w/GCJ7VU3PZ0.jpg",
          price: 340,
          quantity: 0,
        },
      ],
    };
  },
};
</script>

<style scoped>
.home-panel {
  vertical-align: center;
  font-size: 20;
  margin: 15;
}

.description-label {
  margin-bottom: 15;
}
</style>