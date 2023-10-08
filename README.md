# _Angular-Notlar_

## ~~~
### _Liste Sıralama_

##### _Bir tabloya ait verileri seçilen sütun verilerine göre sıralama işlemi_

_dynamicTable öğesi içerisinde sütun başlıklarını tutan dynamicTableColumns listesi ve bu sütunlara ait satır verilerini tutan dynamicTableDatas matriksi bulunduruyor. cloneDatas = [...this._dynamicTable.dynamicTableDatas] yöntemi ile asıl veri matriksi kopyalanıyor ve yapılan sıralama işlemi sırasında asıl veri matriksinde bir değişiklik olmaması sağlanıyor. if(/^\d+$/.test(cloneDatas[0].itemList[selectedColumnIdx].value.slice(-4))) kontrolü ile seçilen sütun verilerinin sayısal olup olmadığı kontrol ediliyor ve bu kontrole göre sıralama işlemi yapılıyor. Son olarak, hatasız ve eksiksiz bir sıralama işlemi için trim() ve replace(/\s+/g, '') yöntemleri ile sıralanacak veriler gereksiz boşluklardan temizleniyor._

```ruby
onSortSelectedColumn(columnName: string) {

        const selectedColumnIdx = this._dynamicTable.dynamicTableColumns.findIndex(q => q.name == columnName);

        const cloneDatas = [...this._dynamicTable.dynamicTableDatas];

        this.isSortedData = !this.isSortedData;
        if(/^\d+$/.test(cloneDatas[0].itemList[selectedColumnIdx].value.slice(-4)))
        {
            if (this.isSortedData) {
                cloneDatas.sort((n1, n2) => +n1.itemList[selectedColumnIdx].value.trim().replace(/\s+/g, '').slice(-4) - +n2.itemList[selectedColumnIdx].value.trim().replace(/\s+/g, '').slice(-4) );
            }
            else {
                cloneDatas.sort((n1, n2) => +n2.itemList[selectedColumnIdx].value.trim().replace(/\s+/g, '').slice(-4) - +n1.itemList[selectedColumnIdx].value.trim().replace(/\s+/g, '').slice(-4) );
            }
        }
        else
        {
            if (this.isSortedData) {
                cloneDatas.sort((n1, n2) => {
                    
                    // if (n1.itemList[selectedColumnIdx].value > n2.itemList[selectedColumnIdx].value) {
                    // return 1;
                    // }
                    // if (n1.itemList[selectedColumnIdx].value < n2.itemList[selectedColumnIdx].value) {
                    // return -1;
                    // }
                    // return 0;
                    return n1.itemList[selectedColumnIdx].value.trim().localeCompare(n2.itemList[selectedColumnIdx].value.trim(), undefined, { sensitivity: 'base' });
                });
            }
            else {
                cloneDatas.sort((n1, n2) => {
                    
                    // if (n1.itemList[selectedColumnIdx].value > n2.itemList[selectedColumnIdx].value) {
                    // return -1;
                    // }
                    // if (n1.itemList[selectedColumnIdx].value < n2.itemList[selectedColumnIdx].value) {
                    // return 1;
                    // }
                    // return 0;
                    return n2.itemList[selectedColumnIdx].value.trim().localeCompare(n1.itemList[selectedColumnIdx].value.trim(), undefined, { sensitivity: 'base' });
                });
            }
        }

        this._screenedDynamicTableDatas = JSON.parse(JSON.stringify(cloneDatas));
    }
```

## ~~~
### _Chart.js - Grafik Ortasına Metin Ekleme_

##### _Chart.js kütüphanesi ile oluşturulan 'doughnut' türündeki grafiğin ortasına metin ekleme işlemi_

```ruby
RenderChart(labeldata: any, realdata: any, colordata: any, type: any, id: any, centertext: string) {
      let delayed;

      const centerText = {
        id: 'centerText',
        afterDraw: (chart: Chart, args: any, options: any) => {
          const { ctx, chartArea: { left, right, top, bottom, width, height } } = chart;
          ctx.save();

          ctx.font = '500 19px Roboto';
          ctx.fillStyle = '#000';
          ctx.textAlign = 'center';
          ctx.fillText(`${centertext}`, width / 2, height / 2 + top, 131);

          ctx.restore();
        },
      };

      const myChart = new Chart(id, {
        type: type,
        data: {
          labels: labeldata,
          datasets: [{
            data: realdata,
            backgroundColor: colordata,
            borderWidth: 2,
          }]
        },
        options: {
          cutout: 70,
          plugins: {
            legend: {
                display: true,
                position: 'top',
                labels: {
                    font: {
                        size: 11,
                    },
                    color: 'black',
                }
            },
          }
        },
        plugins: [centerText],
      });
    }
```

## ~~~
### _..._

##### _..._
