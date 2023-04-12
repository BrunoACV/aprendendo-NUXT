<template>
	<NuxtLayout name="sistema">
		<div>
			<!-- ########## HEADER ########## -->
			<header class="mt-3.5">
				<CabecalhosCabecalho title="Cadastro da Empresa Administradora"
					subtitle="CADASTRO / " lastTitle="EMPRESA ADMINISTRADORA">
					<template v-slot:buttonVoltar>
						<BotoesVoltar>
							<p>Voltar</p>
						</BotoesVoltar>
					</template>
					<template v-slot:buttonSalvar>
						<BotoesSalvar formId="formId">
							<p>Salvar</p>
						</BotoesSalvar>
					</template>
				</CabecalhosCabecalho>
			</header>
			<!-- ########## Forms ########## -->
			<main class="ml-20">
				<form id="formId" @submit.prevent="submitForm">
					<button type="button" @click="dados">teste</button>
					<!-- ########## Dados de identificação ##########  -->
					<div class="mt-[1rem] bg-[#F6F6F6] max-w-[97%] h-[14rem] rounded-[15px] flex flex-col">
						<CabecalhosSubtitulo subtitle="Dados de Identificação da Empresa" />
						<fieldset class="grid grid-cols-[_auto_auto_auto_8rem_10rem] mt-3.5 gap-3">
							<div class="flex flex-col h-10 ml-5">
								<label for="cnpj" class="text-xs text-[#555555]">
									CNPJ <span class="text-[#FF0000]">*</span>
								</label>
								<InputMask 
								mask="99.999.999/9999-99" 
								class="h-10"
								v-model="campos.pj.cnpj"
								@blur="validarCnpj" 
								:class="{ 'is-invalid': cnpjInvalid }" 				
								/>
							</div>
							<div class="flex flex-col h-10">
								<label for="razaoSocial" class="text-xs text-[#555555]">
									Razao Social
									<span class="text-[#FF0000]">*</span>
								</label>
								<InputText id="razaoSocial" v-model="campos.pj.razao_social" class="h-10" required />
							</div>
							<div class="flex flex-col h-10">
								<label for="nomeFantasia" class="text-xs text-[#555555]">
									Nome Fantasia
								</label>
								<InputText id="nomeFantasia" v-model="campos.pj.nome_fantasia" class="h-10" />
							</div>

							<div class="flex flex-col h-10">
								<label for="sigla" class="text-xs text-[#555555]">
									Sigla
								</label>
								<InputText id="sigla" v-model="campos.pj.sigla" class="h-10" />
							</div>
							<div class="flex flex-col h-10">
								<label for="situacao" class="text-xs text-[#555555]">
									Situação <span class="text-[#FF0000]">*</span>
								</label>
								<InputText id="situacao" v-model="campos.pj.situacao" class="h-10 w-24" required />
							</div>
						</fieldset>
						<fieldset class="grid grid-cols-[_20rem_20rem_23.75rem] mt-8 gap-3">
							<div class="ml-5 flex flex-col h-10">
								<label for="naturezaJuridica" class="text-xs text-[#555555]">
									Natureza Jurídica
									<span class="text-[#FF0000]">*</span>
								</label>
								<InputText id="naturezaJuridica" v-model="campos.pj.natureza_juridica" class="h-10" required />
							</div>
							<div class="flex flex-col h-10">
								<label for="responsavelCpf" class="text-xs text-[#555555]">
									Nome do Responsável
								</label>
								<InputText 
								id="responsavel_nome" 
								v-model="campos.responsavel_nome" 
								class="h-10"
								 />
							</div>
							<div class="flex flex-col h-10">
								<label for="responsavelCpf" class="text-xs text-[#555555]">
									CPF do Responsável
								</label>
								<InputMask 
								id="responsavelCpf" 
								mask="999.999.999-99"
								v-model="campos.responsavel_cpf" 
								@blur="validarCpf"
								class="h-10 w-[21.3rem]"
								:class="{ 'is-invalid': cpfInvalid }" 
								 />
							</div>
						</fieldset>
					</div>
					<!-- ########## Dados de endereço ########## -->
					<div class="mt-[1rem] bg-[#F6F6F6] max-w-[97%] h-[10rem] rounded-[15px] flex flex-col">
						<CabecalhosSubtitulo subtitle="Dados de Endereço" />
						<fieldset
							class="justify-start grid grid-cols-[_10rem_13rem_4rem_12rem_10rem_3rem_12rem_10rem] mt-5 gap-3">
							<div class="ml-5 flex flex-col h-10">
								<label for="cep" class="text-xs text-[#555555]">
									CEP <span class="text-[#FF0000]">*</span>
								</label>
								<InputText @blur="getCep" id="cep" v-model="campos.endereco.cep" class="h-10 " required />
							</div>
							<div class="flex flex-col h-10">
								<label for="logradouro" class="text-xs text-[#555555]">
									Logradouro <span class="text-[#FF0000]">*</span>
								</label>
								<InputText id="logradouro" v-model="campos.endereco.logradouro" class="h-10 " required />
							</div>
							<div class="flex flex-col h-10">
								<label for="numero" class="flex text-xs text-[#555555]">
									Número <span class="text-[#FF0000]">*</span>
								</label>
								<InputText inputId="numero" v-model="campos.endereco.numero" class="h-10 flex" required />
							</div>
							<div class="flex flex-col h-10">
								<label for="complemento" class="text-xs text-[#555555]">
									Complemento
									<span class="text-[#FF0000]">*</span>
								</label>
								<InputText id="complemento" v-model="campos.endereco.complemento" class="h-10" required />
							</div>
							<div class="flex flex-col h-10">
								<label for="bairro" class="text-xs text-[#555555]">
									Bairro <span class="text-[#FF0000]">*</span>
								</label>
								<InputText id="bairro" v-model="campos.endereco.bairro" class="h-10" required />
							</div>
							<div class="flex flex-col h-10">
								<label for="uf" class="text-xs text-[#555555]">
									UF <span class="text-[#FF0000]">*</span>
								</label>
								<InputText id="uf" class="h-10" v-model="campos.endereco.uf" required />
							</div>
							<div class="flex flex-col h-10">
								<label for="cidade" class="text-xs text-[#555555]">
									Cidade <span class="text-[#FF0000]">*</span>
								</label>
								<InputText id="cidade" v-model="campos.endereco.cidade" class="h-10" required />
							</div>
							<div class="flex flex-col h-10">
								<label for="pais" class="text-xs text-[#555555]">
									País <span class="text-[#FF0000]">*</span>
								</label>
								<InputText id="pais" v-model="campos.endereco.pais" class="h-10 w-[7.5rem]" required />
							</div>
						</fieldset>
					</div>
					<!-- ########## Dados Contato ########## -->
					<div class="mt-[1rem] bg-[#F6F6F6] max-w-[97%] h-[12rem] rounded-[15px] flex flex-col">
						<CabecalhosSubtitulo subtitle="Dados de Contato" />
						<div class="ml-5 grid grid-cols-[_25rem_8rem_20rem_17rem] items-center gap-3">
							<InputsArquivo />
							<div class="flex flex-col h-10">
								<label for="telefone" class="text-xs text-[#555555]">
									Telefone <span class="text-[#FF0000]">*</span>
								</label>

								<InputText type="tel" id="telefone" v-model="campos.telefone.numero" class="h-10" required />
							</div>
							<div class="flex flex-col h-10">
								<label for="email" class="text-xs text-[#555555]">
									E-mail <span class="text-[#FF0000]">*</span>
								</label>
								<InputText type="email" id="email" v-model="campos.email.email" class="h-10" />
							</div>
							<div class="flex flex-col h-10">
								<label for="url" class="text-xs text-[#555555]">
									Url Site da Empresa
								</label>
								<InputText id="url" v-model="campos.pj.url_site" class="h-10" />
							</div>
						</div>
					</div>
				</form>
			</main>
		</div>
	</NuxtLayout>
</template>

<script setup lang="ts">

// references values inputs
const campos = ref({
	responsavel_cpf: "",
	responsavel_nome:"",
	email: {
		email:"",
	},
	telefone: {
		numero: "",
	},
	pj: {
		cnpj: "",
		sigla: "",
		razao_social: "",
		nome_fantasia: "",
		situacao: "",
		natureza_juridica: "",
		url_logo: "",
		url_site:""
	},
	endereco: {
		cep: "",
		logradouro: "",
		bairro: "",
		cidade: "",
		uf: "",
		pais: "",
		numero: "",
		complemento: "",
		codigo_ibge: ""
	}
}); 
function dados() {
	console.log(campos);
}
// Validação CNPJ
const cnpjInvalid = ref(false);

function validarCNPJ(cnpj) {
    
cnpj = cnpj.replace(/[^\d]+/g,'');

if(cnpj == '') return false;

if (cnpj.length != 14)
  return false;

// Elimina CNPJs invalidos conhecidos
if(/0{14}/.test(cnpj))
        return false

// Valida DVs
let tamanho = cnpj.length - 2
let numeros = cnpj.substring(0,tamanho);
let digitos = cnpj.substring(tamanho);
let soma = 0;
let pos = tamanho - 7;

for (var i = tamanho; i >= 1; i--) {
  soma += numeros.charAt(tamanho - i) * pos--;
  if (pos < 2)
        pos = 9;
}
let resultado = soma % 11 < 2 ? 0 : 11 - soma % 11;
if (resultado != digitos.charAt(0))
    return false;

tamanho = tamanho + 1;
numeros = cnpj.substring(0,tamanho);
soma = 0;
pos = tamanho - 7;
for (i = tamanho; i >= 1; i--) {
  soma += numeros.charAt(tamanho - i) * pos--;
  if (pos < 2)
        pos = 9;
}
resultado = soma % 11 < 2 ? 0 : 11 - soma % 11;
if (resultado != digitos.charAt(1))
      return false;

return true;
}


function validarCnpj() {
  if (campos.value.pj.cnpj) {
    cnpjInvalid.value = !validarCNPJ(campos.value.pj.cnpj);
  } 
}

// Algoritmo validacao cpf 
const cpfInvalid = ref(false); 
function validarCPF(cpf) {
  cpf = cpf.replace(/[^\d]+/g,'');
  if(cpf == '') return false;
  // Elimina CPFs invalidos conhecidos
  if (cpf.length != 11 ||
    cpf == "00000000000" ||
    cpf == "11111111111" ||
    cpf == "22222222222" ||
    cpf == "33333333333" ||
    cpf == "44444444444" ||
    cpf == "55555555555" ||
    cpf == "66666666666" ||
    cpf == "77777777777" ||
    cpf == "88888888888" ||
    cpf == "99999999999")
      return false;
  // Valida 1o digito
  let add = 0;
  for (var i=0; i < 9; i ++)
    add += parseInt(cpf.charAt(i)) * (10 - i);
  let rev = 11 - (add % 11);
  if (rev == 10 || rev == 11)
    rev = 0;
  if (rev != parseInt(cpf.charAt(9)))
    return false;
  // Valida 2o digito
  add = 0;
  for (var i = 0; i < 10; i ++)
    add += parseInt(cpf.charAt(i)) * (11 - i);
  rev = 11 - (add % 11);
  if (rev == 10 || rev == 11)
    rev = 0;
  if (rev != parseInt(cpf.charAt(10)))
    return false;
  return true;
}
function validarCpf() {
  if (campos.value.responsavel_cpf) {
    cpfInvalid.value = !validarCPF(campos.value.responsavel_cpf);
  } else {
	
  }
}
// API CEP (Apos inserir o cep completa os inputs)
async function getCep() {
	const cep = campos.value.endereco.cep;
	const url = `http://viacep.com.br/ws/${cep}/json/`;
	await fetch(url, {
		method:'GET',
	})
		.then(res => res.json())
		.then(response => {
			campos.value.endereco.bairro = response.bairro;
			campos.value.endereco.cidade = response.localidade;
			campos.value.endereco.logradouro = response.logradouro;
			campos.value.endereco.uf = response.uf 
			campos.value.endereco.codigo_ibge = response.ibge;
		})
		.catch(error => console.error(error));
}

// Enviar requisição para api
async function submitForm() {
	let url = "http://127.0.0.1:8000/api/empresa/create";
	
    if(validarCNPJ(campos.value.pj.cnpj) && validarCPF(campos.value.responsavel_cpf)) {
        await $fetch(url, {
		method:"POST",
		body:JSON.stringify(campos)
	})
    } else {
        alert('Erro')
    }
}
</script>

<style scoped>
.is-invalid {
	border: 1px solid rgb(207, 38, 38);
}
</style>