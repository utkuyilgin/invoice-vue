<template>
    <section class="bg-gray-900 w-1/3 mx-auto mt-10 p-2 px-5 rounded-md shadow-2xl">
        <!-- FAtura Bilgileri -->
        <contact-info :contact="state.contact"/>
        <div class="mt-5">
            <heading title="Fatura Kalemleri"/>
            <invoice-items :items="state.items" :AddInvoiceItem="AddInvoiceItem"/>
        </div>
        <!-- Summary -->
        <invoice-summary :items="state.items"/>
        <hr class="bg-gradient-to-r h-[1px] border-none from-gray-700 mt-5"/>
        <div class="actionbar text-right my-5">
            <button @click="onSubmit" class="purple-button">Kaydet</button>
        </div>
    </section>
</template>

<script setup>
import InvoiceItems from "./invoiceItems.vue";
import InvoiceSummary from "./invoiceSummary.vue";
import ContactInfo from "./contactInfo.vue"
import {reactive, provide} from "vue";

const props = defineProps({
    saveInvoice: Function
})
const state = reactive({
    contact: {
        contact_name: null,
        email: null,
        city: null,
        country: null,
        zipcode: null,
    },
    items: [],
});
const AddInvoiceItem = () => {
    state.items.push({
        id: new Date(),
        name: null,
        quantity: null,
        unit_price: 0.0,
        total_price: 0.0,
    })
};
const DeleteInvoiceItem = (invoiceItem) => {
    state.items = state.items.filter(i => i.id !== invoiceItem.id)
};
const onSubmit = () => {
    props.saveInvoice({ ...state });
}
provide("DeleteInvoiceItem", DeleteInvoiceItem);
</script>
