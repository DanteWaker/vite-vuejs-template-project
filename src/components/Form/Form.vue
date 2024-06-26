<script setup lang="ts">
import Card from '../ui/card/Card.vue';
import { toTypedSchema } from '@vee-validate/zod'
import * as z from 'zod'
import { useForm } from 'vee-validate';
import {
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage
} from '@/components/ui/form'
import { Input } from '../ui/input';
import { CardHeader } from '../ui/card';
import { Button } from '../ui/button';

import { vMaska } from "maska/vue"

function removerPontosEVirgulas(str: string) {
  return str.replace(/[.,]/g, '');
}

const options = {
  mask: "9 99#,##",
  reversed: true,
  tokens: {
    0: { pattern: /[0-9]/, optional: true },
    9: { pattern: /[0-9]/, repeated: true },
  }
};

const formSchema = toTypedSchema(z.object({
  pessoaFisica: z.string(),
  pessoaJuridica: z.string(),
  passagens: z.string(),
  indExecutiva: z.boolean(),
  valorLojas: z.string(),
  valorGestao: z.string(),
}))

const form = useForm({
  validationSchema: formSchema,
})

const onSubmit = form.handleSubmit((values) => {
  console.log('Form submitted!', values)
  console.log(removerPontosEVirgulas(values.pessoaFisica))
})
</script>


<template>
  <Card class="p-5 w-[40rem] border-[1px] border-primary">
    <CardHeader class="text-primary font-bold text-xl">Simulador de Custos</CardHeader>
    <form class="w-2/3 space-y-6 w-full p-6 pt-0 gap-2" @submit="onSubmit">

      <div class="flex gap-2">
        <FormField v-slot="{ componentField }" name="pessoaFisica">
          <FormItem class="w-[50%]">
            <FormLabel class="text-s">Valor gasto na Pessoa Física</FormLabel>
            <FormControl>
              <div class="flex">
                <div class="h-10 w-[15%] bg-background flex justify-center items-center font-bold">R$</div>
                <Input class=" w-[85%]" v-maska='options' type="text" placeholder="Digite o valor gasto"
                  v-bind="componentField" />
              </div>
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
        <FormField v-slot="{ componentField }" name="pessoaJuridica">
          <FormItem class="w-[50%]">
            <FormLabel class="text-s">Valor gasto na Pessoa Jurídica</FormLabel>
            <FormControl>
              <div class="flex">
                <div class="h-10 w-[15%] bg-background flex justify-center items-center font-bold">R$</div>
                <Input class=" w-[85%]" v-maska='options' type="text" placeholder="Digite o valor gasto"
                  v-bind="componentField" />
              </div>
            </FormControl>
            <FormMessage />
          </FormItem>
        </FormField>
      </div>
      <Button type="submit" class="text-foreground rounded-none hover:rounded-md">
        Calcular
      </Button>

    </form>
  </Card>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
