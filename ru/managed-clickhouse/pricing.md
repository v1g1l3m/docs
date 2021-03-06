---
editable: false
---

# Правила тарификации для {{ mch-short-name }}

{% include [currency-choice](../_includes/pricing/currency-choice.md) %}

{% include [pricing-status.md](../_includes/mdb/pricing-status.md) %}

{% include [pricing-status-warning.md](../_includes/mdb/pricing-status-warning.md) %}


## Из чего складывается стоимость использования {{ mch-short-name }} {#rules}

Расчет стоимости использования {{ mch-name }} учитывает:

* тип и объем хранилища (дискового пространства);

* вычислительные ресурсы, выделенные хостам кластера (в том числе хостам {{ ZK }});

* настройки и количество резервных копий;

* объем исходящего трафика из Яндекс.Облака в интернет.


{% include [pricing-gb-size](../_includes/pricing-gb-size.md) %}


### Использование хостов БД {#rules-hosts-uptime}

Стоимость начисляется за каждый час работы хоста в соответствии с выделенными для него вычислительными ресурсами. Поддерживаемые конфигурации ресурсов приведены в разделе [{#T}](concepts/instance-types.md), цены за использование vCPU и RAM — в разделе [Цены](#prices).

Вы можете выбрать класс хостов как для хостов {{ CH }}, так и для хостов ZooKeeper (в соответствии с ожидаемой нагрузкой реплицирования).

{% note important %}

При создании кластера с 2 и более хостами {{ CH }} автоматически создается 3 хоста {{ ZK }} минимального класса, которые обеспечивают репликацию и отказоустойчивость кластера.

{% endnote %}

Минимальная единица тарификации — час (например, стоимость 1,5 часов работы хоста равна стоимости 2 часов). Время, когда хост СУБД или {{ ZK }} не может выполнять свои основные функции, не тарифицируется.


### Использование дискового пространства {#rules-storage}

Оплачивается:

* Объем хранилища, выделенный для кластеров БД.

    * Хранилище на быстрых локальных дисках (`local-ssd`) можно заказывать только для кластеров более чем с 2 хостами, с шагом 100 ГБ.


* Объем, занимаемый резервными копиями баз данных сверх заданного хранилища для кластера.

    * Хранение резервных копий не тарифицируется, пока сумма размера БД и всех резервных копий остается меньше выбранного объема хранилища.

    * При автоматическом резервном копировании {{ mch-short-name }} не создает новую копию, а сохраняет изменения БД по сравнению с предыдущей копией. Поэтому потребление хранилища автоматическими резервными копиями растет только пропорционально объему изменений.

    * Количество хостов кластера не влияет на объем хранилища и, соответственно, на бесплатный объем резервных копий.

Цена указывается за 1 месяц использования.  Минимальная единица тарификации — ГБ в час (например, стоимость хранения 1 ГБ в течение 1,5 часов равна стоимости хранения в течение 2 часов).


### Пример расчета стоимости кластера {#example}

Например, вы создали кластер:

* с 3 хостами {{ CH }} с классом хостов `s1.micro` (2 vCPU, 8 ГБ RAM),
* с 3 автоматически созданными хостами {{ ZK }} класса `s1.nano` (1 vCPU, 4 ГБ RAM).
* c 100 ГБ стандартного сетевого хранилища.

Стоимость часа работы хостов: `3 × (2 × 1,43 ₽ + 8 × 0,33 ₽) + 3 × (1 × 0,89 ₽ + 4 × 0,2 ₽) = 21,57 ₽`

Общая стоимость кластера в месяц (хосты и хранилище): `720 × 21,57 ₽ + 100 × 2,2881 ₽ = 15 759,21 ₽`


## Цены {#prices}


### Вычислительные ресурсы хостов {{ CH }} {#prices-clickhouse}

Ресурс | Цена за 1 час, вкл. НДС
----- | -----
**Intel Broadwell** |
5% vCPU | 0,02 ₽
20% vCPU | 0,26 ₽
50% vCPU | 0,43 ₽
100% vCPU | 1,43 ₽
RAM (за 1 ГБ) | 0,33 ₽
**Intel Cascade Lake** |
5% vCPU | 0,02 ₽
20% vCPU | 0,26 ₽
50% vCPU | 0,43 ₽
100% vCPU | 1,20 ₽
RAM (за 1 ГБ) | 0,33 ₽


### Вычислительные ресурсы хостов {{ ZK }} {#prices-zookeeper}

Ресурс | Цена за 1 час, вкл. НДС
----- | -----
**Intel Broadwell** |
5% vCPU | 0,15 ₽
20% vCPU | 0,35 ₽
50% vCPU | 0,49 ₽
100% vCPU | 0,89 ₽
RAM (за 1 ГБ) | 0,20 ₽
**Intel Cascade Lake** |
5% vCPU | 0,15 ₽
20% vCPU | 0,35 ₽
50% vCPU | 0,49 ₽
100% vCPU | 0,76 ₽
RAM (за 1 ГБ) | 0,20 ₽


### Хранилище и резервные копии {#prices-storage}

Услуга | Цена за ГБ в месяц, вкл. НДС
----- | -----
Стандартное сетевое хранилище  | 2,2881 ₽ |
Быстрое сетевое хранилище  | 8,1356 ₽ |
Быстрое локальное хранилище  | 8,1356 ₽ |
Резервные копии сверх размера хранилища  | 2,5424 ₽


### Исходящий трафик {#prices-traffic}

{% include notitle [pricing-egress-traffic](../_includes/pricing/pricing-egress-traffic.md) %}


## Расчетные цены для классов хостов {#calculated-host-price}

Цены за время работы хостов рассчитаны исходя из конфигураций [классов хостов](concepts/instance-types.md) и приведенных выше цен за использование vCPU и RAM для соответствующей платформы. Чтобы точно рассчитать стоимость нужного кластера, воспользуйтесь [калькулятором](https://cloud.yandex.ru/services/managed-clickhouse#calculator).

{% include [host-class-price-alert](../_includes/mdb/pricing-host-class-alert.md) %}


### Хосты {{ CH }} {#calculated-clickhouse}

{% list tabs %}

- За месяц работы хоста

  Из расчета 720 часов в месяц, округлено до целых рублей.

  Класс хостов | Цена за месяц, вкл. НДС
  ----- | -----
  **Intel Broadwell** |
  {{ b1-nano }} | 504 ₽
  {{ b1-micro }} | 850 ₽
  {{ b1-medium }}  | 1 570 ₽
  {{ s1-nano }} | 1 980 ₽
  {{ s1-micro }} | 3 960 ₽
  {{ s1-small }} | 7 920 ₽
  {{ s1-medium }}| 15 840 ₽
  {{ s1-large }} | 31 680 ₽
  {{ s1-xlarge }} | 63 360 ₽
  **Intel Cascade Lake** |
  {{ b2-nano }}| 504 ₽
  {{ b2-micro }} | 850 ₽
  {{ b2-medium }} | 1 570 ₽
  {{ m2-micro }} | 5 540 ₽
  {{ m2-small }} | 11 080 ₽
  {{ m2-medium }}| 16 620 ₽
  {{ m2-large }} | 22 160 ₽
  {{ m2-xlarge }} | 33 240 ₽
  {{ m2-2xlarge }} | 44 320 ₽
  {{ m2-3xlarge }} | 66 479 ₽
  {{ m2-4xlarge }} | 88 639 ₽
  {{ m2-5xlarge }} | 110 798 ₽
  {{ m2-6xlarge }} | 132 958 ₽
  {{ s2-micro }} | 3 629 ₽
  {{ s2-small }} | 7 258 ₽
  {{ s2-medium }} | 14 515 ₽
  {{ s2-large }} | 21 773 ₽
  {{ s2-xlarge }} | 29 030 ₽
  {{ s2-2xlarge }} | 43 546 ₽
  {{ s2-3xlarge }} | 58 061 ₽
  {{ s2-4xlarge }} | 72 576 ₽
  {{ s2-5xlarge }} | 87 091 ₽
  {{ s2-6xlarge }} | 116 122 ₽

- За 1 час работы хоста

  Класс хостов | Цена за 1 час, вкл. НДС
  ----- | -----
  **Intel Broadwell** |
  {{ b1-nano }}| 0,70 ₽
  {{ b1-micro }} | 1,18 ₽
  {{ b1-medium }} | 2,18 ₽
  {{ s1-nano }} | 2,75 ₽
  {{ s1-micro }} | 5,50 ₽
  {{ s1-small }} | 11,00 ₽
  {{ s1-medium }} | 22,00 ₽ |
  {{ s1-large }} | 44,00 ₽ |
  {{ s1-xlarge }} | 88,00 ₽ |
  **Intel Cascade Lake** | |
  {{ b2-nano }}| 0,70 ₽
  {{ b2-micro }} | 1,18 ₽
  {{ b2-medium }} | 2,18 ₽
  {{ m2-micro }} | 7,68 ₽
  {{ m2-small }} | 15,36 ₽
  {{ m2-medium }}| 23,04 ₽
  {{ m2-large }} | 30,72 ₽
  {{ m2-xlarge }} | 46,08 ₽
  {{ m2-2xlarge }} | 61,44 ₽
  {{ m2-3xlarge }} | 92,16 ₽
  {{ m2-4xlarge }} | 122,88 ₽
  {{ m2-5xlarge }} | 153,60 ₽
  {{ m2-6xlarge }} | 184,32 ₽
  {{ s2-micro }} | 5,04 ₽
  {{ s2-small }} | 10,08 ₽
  {{ s2-medium }} | 20,16 ₽
  {{ s2-large }} | 30,24 ₽
  {{ s2-xlarge }} | 40,32 ₽
  {{ s2-2xlarge }} | 60,48 ₽
  {{ s2-3xlarge }} | 80,64 ₽
  {{ s2-4xlarge }} | 100,80 ₽
  {{ s2-5xlarge }} | 120,96 ₽
  {{ s2-6xlarge }} | 161,28 ₽

{% endlist %}


### Хосты ZooKeeper {#prices-zookeeper}

{% list tabs %}

- За месяц работы хоста

  Класс хостов | Цена за месяц, вкл. НДС
  ----- | -----
  **Intel Broadwell** |
  {{ b1-nano }} | 504 ₽
  {{ b1-micro }}  | 792 ₽
  {{ b1-medium }}  | 1 282 ₽
  {{ s1-nano }} | 1 217 ₽
  {{ s1-micro }}  | 2 434 ₽
  {{ s1-small }}  | 4 867 ₽
  {{ s1-medium }}  | 9 734 ₽
  {{ s1-large }}  | 19 469 ₽
  {{ s1-xlarge }}  | 38 938 ₽
  **Intel Cascade Lake** |
  {{ b2-nano }} | 504 ₽
  {{ b2-micro }}  | 792 ₽
  {{ b2-medium }}  | 1 282 ₽
  {{ m2-micro }} | 3 398 ₽
  {{ m2-small }} | 6 797 ₽
  {{ m2-medium }}| 10 195 ₽
  {{ m2-large }} | 13 594 ₽
  {{ m2-xlarge }} | 20 390 ₽
  {{ m2-2xlarge }} | 27 187 ₽
  {{ m2-3xlarge }} | 40 781 ₽
  {{ m2-4xlarge }} | 54 374 ₽
  {{ m2-5xlarge }} | 67 968 ₽
  {{ m2-6xlarge }} | 81 562 ₽
  {{ s2-micro }}  | 2 246 ₽
  {{ s2-small }}  | 4 493 ₽
  {{ s2-medium }}  | 8 986 ₽
  {{ s2-large }}  | 13 478 ₽
  {{ s2-xlarge }}  | 17 971 ₽
  {{ s2-2xlarge }}  | 26 957 ₽
  {{ s2-3xlarge }}  | 35 942 ₽
  {{ s2-4xlarge }} | 44 928 ₽
  {{ s2-5xlarge }} | 53 914 ₽
  {{ s2-6xlarge }} | 71 885 ₽

- За 1 час работы хоста

  Класс хостов | Цена за 1 час, вкл. НДС
  ----- | -----
  **Intel Broadwell** |
  {{ b1-nano }} | 0,70 ₽
  {{ b1-micro }}  | 1,10 ₽
  {{ b1-medium }}  | 1,78 ₽
  {{ s1-nano }}  | 1,69 ₽ |
  {{ s1-micro }}  | 3,38 ₽ |
  {{ s1-small }}  | 6,76 ₽ |
  {{ s1-medium }}  | 13,52 ₽ |
  {{ s1-large }}  | 27,04 ₽ |
  {{ s1-xlarge }}  | 54,08 ₽ |
  **Intel Cascade Lake** |
  {{ b2-nano }} | 0,70 ₽
  {{ b2-micro }}  | 1,10 ₽
  {{ b2-medium }}  | 1,78 ₽
  {{ m2-micro }} | 4,72 ₽
  {{ m2-small }} | 9,44 ₽
  {{ m2-medium }}| 14,16 ₽
  {{ m2-large }} | 18,88 ₽
  {{ m2-xlarge }} | 28,32 ₽
  {{ m2-2xlarge }} | 37,76 ₽
  {{ m2-3xlarge }} | 56,64 ₽
  {{ m2-4xlarge }} | 75,52 ₽
  {{ m2-5xlarge }} | 94,40 ₽
  {{ m2-6xlarge }} | 113,28 ₽
  {{ s2-micro }}  | 3,12 ₽
  {{ s2-small }}  | 6,24 ₽
  {{ s2-medium }}  | 12,48 ₽
  {{ s2-large }}  | 18,72 ₽
  {{ s2-xlarge }}  | 24,96 ₽
  {{ s2-2xlarge }}  | 37,44 ₽
  {{ s2-3xlarge }}  | 49,92 ₽
  {{ s2-4xlarge }} | 62,40 ₽
  {{ s2-5xlarge }} | 74,88 ₽
  {{ s2-6xlarge }} | 99,84 ₽

{% endlist %}


