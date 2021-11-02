<template>
  <section>
    <h2 class="my-2 font-weight-light text-center">Images</h2>
    <form class="input-group">
      <input type="text" class="form-control" v-model="imgInput" /><button
        class="btn btn-lg btn-primary bi bi-upload"
        @click.prevent="addImg"
      ></button>
    </form>

    <div class="my-2 images-small">
      <span v-for="img in imgArray">
        <img :src="img.img" :alt="img.img" @dblclick="deleteImg(img)" />
      </span>
    </div>
    <button class="btn btn-sm btn-outline-warning btn-absolut" @click="copy">
      copy
    </button>
    <div class="input-group">
      <textarea
        class="bg-dark text-warning form-control areaImage my-2"
        v-model="imgArea"
        cols="30"
        rows="10"
        id="imageArea"
      ></textarea>
    </div>
  </section>
</template>
<script>
import { ref } from "vue";
export default {
  name: "Image",
  props: {},
  setup() {
    const imgInput = ref(null);
    const imgArea = ref(null);
    const imgArray = ref([]);
    const startOfPage = `<!doctype html>
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:v="urn:schemas-microsoft-com:vml" xmlns:o="urn:schemas-microsoft-com:office:office">

<head>
  <title>
  </title>
  <!--[if !mso]><!-->
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <!--<![endif]-->
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style type="text/css">
    #outlook a {
      padding: 0;
    }

    body {
      margin: 0;
      padding: 0;
      -webkit-text-size-adjust: 100%;
      -ms-text-size-adjust: 100%;
    }

    table,
    td {
      border-collapse: collapse;
      mso-table-lspace: 0pt;
      mso-table-rspace: 0pt;
    }

    img {
      width: 100%;
      display: block;
      border: 0;
      height: auto;
      line-height: 100%;
      outline: none;
      text-decoration: none;
      -ms-interpolation-mode: bicubic;
    }

    p {
      display: block;
      margin: 13px 0;
    }

  </style>
  <!--[if mso]>
        <noscript>
        <xml>
        <o:OfficeDocumentSettings>
          <o:AllowPNG/>
          <o:PixelsPerInch>96</o:PixelsPerInch>
        </o:OfficeDocumentSettings>
        </xml>
        </noscript>
        <![endif]-->
  <!--[if lte mso 11]>
        <style type="text/css">
          .mj-outlook-group-fix { width:100% !important; }
        </style>
        <![endif]-->
  <style type="text/css">
    @media only screen and (min-width:480px) {
      .mj-column-per-100 {
        width: 100% !important;
        max-width: 100%;
      }
    }

  </style>
  <style media="screen and (min-width:480px)">
    .moz-text-html .mj-column-per-100 {
      width: 100% !important;
      max-width: 100%;
    }

  </style>
  <style type="text/css">
    @media only screen and (max-width:480px) {
      table.mj-full-width-mobile {
        width: 100% !important;
      }

      td.mj-full-width-mobile {
        width: auto !important;
      }
    }

  </style>
</head>

<body style="word-spacing:normal;">
  <div style="">`;

    const startOfImg = `<table align="center" border="0" cellpadding="0" cellspacing="0" role="presentation" style="background:#ffffff;background-color:#ffffff;width:100%;">
      <tbody>
        <tr>
          <td>
            <!--[if mso | IE]><table align="center" border="0" cellpadding="0" cellspacing="0" class="" role="presentation" style="width:640px;" width="640" bgcolor="#ffffff" ><tr><td style="line-height:0px;font-size:0px;mso-line-height-rule:exactly;"><![endif]-->
            <div style="margin:0px auto;max-width:640px;">
              <table align="center" border="0" cellpadding="0" cellspacing="0" role="presentation" style="width:100%;">
                <tbody>
                  <tr>
                    <td style="direction:ltr;font-size:0px;padding:0px;text-align:center;">
                      <!--[if mso | IE]><table role="presentation" border="0" cellpadding="0" cellspacing="0"><tr><td align="center" class="" style="vertical-align:top;width:640px;" ><![endif]-->
                      <div class="mj-column-per-100 mj-outlook-group-fix" style="font-size:0px;text-align:left;direction:ltr;display:inline-block;vertical-align:top;width:100%;">
                        <table border="0" cellpadding="0" cellspacing="0" role="presentation" width="100%">
                          <tbody>
                            <tr>
                              <td style="vertical-align:top;padding:0px;">
                                <table border="0" cellpadding="0" cellspacing="0" role="presentation" style="" width="100%">
                                  <tbody>
                                    <tr>
                                      <td align="center" style="font-size:0px;padding:0px;word-break:break-word;">
                                        <table border="0" cellpadding="0" cellspacing="0" role="presentation" style="border-collapse:collapse;border-spacing:0px;">
                                          <tbody>
                                            <tr>
                                              <td style="width:640px;">
                                                <img height="auto" src="`;

    const endOfImg = `" style="border:0;display:block;outline:none;text-decoration:none;height:auto;width:100%;font-size:13px;" width="640" />
                                              </td>
                                            </tr>
                                          </tbody>
                                        </table>
                                      </td>
                                    </tr>
                                  </tbody>
                                </table>
                              </td>
                            </tr>
                          </tbody>
                        </table>
                      </div>
                      <!--[if mso | IE]></td></tr></table><![endif]-->
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <!--[if mso | IE]></td></tr></table><![endif]-->
          </td>
        </tr>
      </tbody>
    </table>`;

    const endOfPage = ` </div>
</body>

</html>`;

    const addImg = () => {
      
      if (imgInput.value != null && !imgArray.value.filter(i=>i.img==imgInput.value).length) {
        imgArray.value = [
          ...imgArray.value,
          { id: Date.now(), img: imgInput.value },
        ];
        imgInput.value = null;
      }

      let result = startOfPage + "\n" ;
      for (let index = 0; index < imgArray.value.length; index++) {
        const element = imgArray.value[index];
        result +=  startOfImg + element.img +  endOfImg;
      }
      imgArea.value = result + "\n"+ endOfPage ;
    };
    const deleteImg = (elemnt) => {
      
      imgArray.value = imgArray.value.filter((img) => img.id != elemnt.id);
      addImg();
    };
    const copy = () => {
      navigator.clipboard.writeText(imgArea.value);
    };
    return {
      imgArray,
      imgInput,
      imgArea,

      addImg,
      deleteImg,
      copy,
    };
  },
};
</script>
