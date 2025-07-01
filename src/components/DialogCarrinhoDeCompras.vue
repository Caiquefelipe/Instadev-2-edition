<script lang="ts" setup>
import { QDialog } from 'quasar'
import { Produto } from 'src/interfaces/Produtos'
import { ref } from 'vue'

const props = defineProps<{
    itens: Produto[]
}>()

const emit = defineEmits<{
    (e: 'remover', index: number): void
    (e: 'adiconar', index: number): void
}>()

const dialog = ref<QDialog>()
console.log(props.itens, 'itens no carrinho');
function show() {
    dialog.value?.show()
}
function hide() {
    dialog.value?.hide()
}

defineExpose({ show })
</script>

<template>
    <q-dialog ref="dialog" persistent>
        <q-card style="width: 100vw;">
            <div class="flex justify-between items-center">
                <q-card-section class="text-h6">Carrinho de Compras</q-card-section>
                <q-btn flat icon="close" @click="hide()" />
            </div>

            <q-separator />

            <q-card-section>
                <div v-if="props.itens.length">
                    <q-list>
                        <q-item v-for="(item, index) in props.itens" :key="item.id">
                            <q-item-section>
                                <div class="text-h6">{{ item.nome }}</div>
                                <div>Preço: R$ {{ item.preco.toFixed(2) }}</div>
                            </q-item-section>

                            <q-item-section side>
                                <div v-if="item.quantidade == 1">
                                    <q-btn flat color="red" icon="fa-solid fa-trash" @click="emit('remover', index)" />
                                    {{ item.quantidade }}
                                </div>
                                <div v-else>
                                    <q-btn flat color="red" icon="remove_circle" @click="emit('remover', index)" />
                                    {{ item.quantidade }}
                                </div>

                            </q-item-section>
                            <q-btn flat color="green" icon="add_circle" @click="emit('adiconar', index)" />

                        </q-item>
                    </q-list>
                </div>

                <div v-else>
                    <div class="column items-center text-grey">
                        <q-icon name="remove_shopping_cart" size="64px" class="q-mb-md" color="grey-5" />
                        <div class="text-subtitle1">Seu carrinho está vazio</div>
                        <div class="text-caption">Adicione produtos para começar suas compras!</div>
                    </div>
                </div>
            </q-card-section>
        </q-card>
    </q-dialog>
</template>

<style scoped></style>
