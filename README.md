## Vercel 部署说明
1：点击Use this template ➡ Create a new repostory 创建一个私密项目, 名称随意

2: 修改index.js文件里的环境变量保存，不需要的留空

3: 用AI生成一个纯html的网页替换`index.thml`伪装网页

4：打开vercel连接项目部署
  - 打开 Vercel 控制台
  - 点击 “New Project”
  - Import该项目
  - 选择默认配置, Install Command打开，设置为`npm install`
  - 点击 “Deploy”, 等待部署完成

5：部署完成后确认没问题，将反代后的域名填写到`index.js`的DOMAIN环境变量里, 然后用 https://www.jshaman.com/index.html 混淆替换后保存

6：获取订阅信息，修改优选域名使用(节点里的address字段),可以自行套订阅器，比如：https://sub.eooce.xx.kg


## Vercel 常用地区代码对照表

下面是常见 Vercel `regions` 代码对应关系：

| 地区代码 | 城市/区域 | 国家/地区 | 中文说明 |
|---|---|---|---|
| `hnd1` | Tokyo | Japan | 日本东京 |
| `kix1` | Osaka | Japan | 日本大阪 |
| `icn1` | Seoul | South Korea | 韩国首尔 |
| `hkg1` | Hong Kong | Hong Kong | 中国香港 |
| `sin1` | Singapore | Singapore | 新加坡 |
| `syd1` | Sydney | Australia | 澳大利亚悉尼 |
| `bom1` | Mumbai | India | 印度孟买 |
| `del1` | New Delhi | India | 印度新德里 |
| `fra1` | Frankfurt | Germany | 德国法兰克福 |
| `lhr1` | London | United Kingdom | 英国伦敦 |
| `cdg1` | Paris | France | 法国巴黎 |
| `ams1` | Amsterdam | Netherlands | 荷兰阿姆斯特丹 |
| `mad1` | Madrid | Spain | 西班牙马德里 |
| `dub1` | Dublin | Ireland | 爱尔兰都柏林 |
| `dxb1` | Dubai | United Arab Emirates | 阿联酋迪拜 |
| `jnb1` | Johannesburg | South Africa | 南非约翰内斯堡 |
| `cpt1` | Cape Town | South Africa | 南非开普敦 |
| `gru1` | São Paulo | Brazil | 巴西圣保罗 |
| `sfo1` | San Francisco | United States | 美国旧金山 |
| `iad1` | Washington, D.C. | United States | 美国华盛顿特区 |
| `pdx1` | Portland | United States | 美国波特兰 |
| `mia1` | Miami | United States | 美国迈阿密 |
| `ewr1` | Newark | United States | 美国纽瓦克 |
| `yul1` | Montréal | Canada | 加拿大蒙特利尔 |

---

## 注意事项

部署成功后，访问项目地址，确认：

1. 页面能正常访问
2. 生成的订阅链接能获取到节点
3. vervel分配的域名已被墙，无法使用直连节点
4. 哪吒不亮，不用填写


## 使用cloudflare的workers或snippets反代项目域名，给节点套CDN加速
```bash
export default {
    async fetch(request, env) {
        let url = new URL(request.url);
        if (url.pathname.startsWith('/')) {
            var arrStr = [
                'xxx-xxx.vercel.app',   // 此处单引号里填写你的vercel分配的域名，不包含https://
            ];
            url.protocol = 'https:'
            url.hostname = getRandomArray(arrStr)
            let new_request = new Request(url, request);
            return fetch(new_request);
        }
        return env.ASSETS.fetch(request);
    },
};
function getRandomArray(array) {
  const randomIndex = Math.floor(Math.random() * array.length);
  return array[randomIndex];
}
```
