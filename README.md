<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>PDF to Text Converter</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.12.313/pdf.min.js"></script>
  <style>
    body { font-family: Arial; padding: 20px; }
    h2 { color: #333; }
    #pdfText { white-space: pre-wrap; background: #f4f4f4; padding: 10px; border: 1px solid #ccc; }
  </style>
</head>
<body>
  <h2>Upload a PDF File</h2>
  <input type="file" id="pdfInput" accept="application/pdf" />
  <h3>Extracted Text:</h3>
  <div id="pdfText">Nothing extracted yet...</div>

  <script>
    document.getElementById('pdfInput').addEventListener('change', async (e) => {
      const file = e.target.files[0];
      if (!file) return;

      const fileReader = new FileReader();
      fileReader.onload = async function () {
        const typedarray = new Uint8Array(this.result);
        const pdf = await pdfjsLib.getDocument({ data: typedarray }).promise;
        let fullText = '';
        for (let i = 1; i <= pdf.numPages; i++) {
          const page = await pdf.getPage(i);
          const content = await page.getTextContent();
          const pageText = content.items.map(item => item.str).join(' ');
          fullText += pageText + '\n\n';
        }
        document.getElementById('pdfText').textContent = fullText;
      };
      fileReader.readAsArrayBuffer(file);
    });
  </script>
</body>
</html>
