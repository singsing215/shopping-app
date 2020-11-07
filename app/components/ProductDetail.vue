<template>
  <Page>
    <ScrollView orientation="vertical">
      <StackLayout orientation="vertical" margin="10" class="form">
        <Image :src="selectedProduct.image" height="300" stretch="aspectFill" />
        <Label :text="selectedProduct.name" margin="10" class="h2" />
        <Label :text="selectedProduct.desc" class="body" />
        <Label
          :text="'Unit Price: $' + selectedProduct.price"
          margin="10"
          class="text-right"
        />
        <Label text="Quantity" class="h3" />
        <TextField
          v-model="textFieldValue"
          hint="Enter text..."
          class="input input-border"
          keyboardType="phone"
        />
        <Label
          :text="'SubTotal: $' + textFieldValue * selectedProduct.price"
          class="h3"
          margin="10"
        />
        <Button
          text="Add to Cart"
          @tap="onButtonTap"
          class="btn btn-primary btn-rounded-lg"
        />
      </StackLayout>
    </ScrollView>
  </Page>
</template>

<script>
export default {
  props: ["selectedProduct", "$delegate"],

  methods: {
    onButtonTap() {
      console.log("Button was pressed");

      var result = confirm({
        title: "Confirm?",
        message: "This item will added to your shopping cart.",
        okButtonText: "Yes",
        cancelButtonText: "Cancel",
      });

      if (result) {
        this.selectedProduct.quantity = parseInt(this.textFieldValue) || 0;
        this.$delegate.updateCart();
      }
    },
  },

  data() {
    return {
      textFieldValue: this.selectedProduct.quantity || "",
    };
  },
};
</script>

<style>
</style>