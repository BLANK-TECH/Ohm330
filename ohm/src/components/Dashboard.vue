<template>
    <div>
        <div id="content">
            <h3>Sample h3 tag</h3>
            <p>Sample pararaph</p>
        </div>
<button @click="downloadPDF">Descargar</button>
    </div>
</template>

<script>
import jsPDF from 'jspdf';

export default {
    methods: {
        downloadPDF() {
            let generate = new jsPDF();
            let source = document.getElementById('content')
            generate.fromHTML(source, 15, 15)
            generate.save('sample-file.pdf')
        }
    }
}
</script>

<style scoped>
    
</style>
