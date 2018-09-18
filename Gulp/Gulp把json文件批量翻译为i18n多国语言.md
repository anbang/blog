百度的翻译还是不给力， 改为Google进行文档翻译了；推荐使用Google, 翻译的更好；

#### 项目使用VUE-I18N做多国语言处理；

运行环境是Electron+vue做的客户端项目；

需求：用一份json文件作为模板，构建出22国语言；每个国家的语言，单独储存在一个json文件内；用的是百度翻译的API

开始的基础文件

任务执行后的文件；

用的百度翻译API(迫于翻译太烂，还是改为谷歌了 - -、)

npm的依赖如下

```json
"devDependencies": {
    "gulp": "^3.9.1",
    "gulp-rename": "^1.2.2",
    "gulp-util": "^3.0.8",
    "md5": "^2.2.1",
    "request": "^2.85.0",
    "through2": "^2.0.3"
}
```

文件夹
```
/i18n/config
/i18n/languges_conf
/i18n/tap-i18n.json
```

## gulp的配置文件

`gulpfile.js`文件如下

```javascript
/**
 *  Translate Chinese json files into other language json files
 *  language : https://cloud.google.com/translate/docs/languages
 */
const gulp = require('gulp'),
    i18n = require('./i18n/config'),
    // languges = require('./i18n/languges'),
    languges = require('./i18n/languges_conf'),
    rename = require('gulp-rename');
// The task of translating files
gulp.task('default', function () {
    for (const languge in languges) {
        // console.log(languge,languges[languge])
        gulp.src('./i18n/tap-i18n.json')
            .pipe(
                // The specific logic of translation
                i18n('', languge)
            )
            .pipe(rename({
                dirname: "i18n",
                basename: languge,
                extname: ".json"
            }))
            //  Post-translation file output file path
            .pipe(gulp.dest('./src/renderer/'));
    }
});

gulp.task('watch', function () {
    gulp.watch('i18n/tap-i18n.json', ['default']);
})
```
## 配置中引用了别的配置 `/i18n/config.js`，内容如下；

```javascript
const { youdao, baidu, google } = require('translation.js');

let through = require('through2'),
    gutil = require('gulp-util');
PluginError = gutil.PluginError;

// Constants, error prompts for use
const PLUGIN_NAME = 'gulp-i18n';

/**
 * Convert json language
 * @param from Translate source data
 * @param to  Translation language
 * @returns {*}
 */
function i18n(from, to) {
    // Create a stream channel to allow each file to pass
    let stream = through.obj(function (file, enc, cb) {
        // console.log(JSON.stringify(file))
        if (file.isStream()) {
            this.emit('error', new PluginError(PLUGIN_NAME, 'Streams are not supported!'));
            return cb();
        }
        if (file.isBuffer()) {
            let json = JSON.parse(file.contents.toString());
            let tJson = '';
            //  Get the data to be translated in the json file
            for (let v in json) {
                if (json[v] instanceof Object) {
                    tJson = getText(json[v], tJson);
                }
                else {
                    tJson += json[v] + "\n";
                }
            }
            if (tJson.length > 2000)
                this.emit('error', new PluginError(PLUGIN_NAME, 'Source file is greater than 2000 words'));
            //Google Translate
            google.translate({
                text: tJson,
                from: from,
                to: to
            }).then(result => {
                // console.log("to:"+to,result.result,result) // The data structure of result is shown below

                /*
                * esult.result = [ 'Nastavení',
                    'CZR',
                    'Domů',
                    'Přenos',
                    'Kontakty',
                    'Nastavení',
                    'Celkem',
                    'Nastavení' ]
                * */
                // let result=result;
                // console.log("内容是",file.contents.toString())
                let content = file.contents.toString().replace(/(:\s*")(\S+)(")/gi, function (match, p1, p2, p3, offset, string) {
                    /*              console.log(
                    // result.raw.sentences,
                                      "【match=>"+match+"】",
                                      "【p1=>"+p1+"】",
                                      "【p2=>"+p2+"】",
                                      "【p3=>"+p3+"】",
                                      "【offset=>"+offset+"】",
                                      "【string=>"+string+"】",
                                      )*/

                    /*
                    result.raw.sentences :[
                        { trans: 'Setting\n', orig: 'Setting\n', backend: 3 },
                        { trans: 'CZR\n', orig: 'CZR\n', backend: 3 },
                        { trans: 'Home\n', orig: '首页\n', backend: 3 },
                        { trans: 'Transfer\n', orig: '转账\n', backend: 3 },
                        { trans: 'Contacts\n', orig: '联系人\n', backend: 3 },
                        { trans: 'Settings\n', orig: '设置\n', backend: 3 },
                        { trans: 'total\n', orig: '合计\n', backend: 2 },
                        { trans: 'Settings', orig: '设置', backend: 3 } ] '
                          【match=>:"设置"】'
                          '【p1=>:"】'
                          '【p2=>设置】'
                          '【p3=>"】'
                          '【offset=>270】'
                          '【string=>{\n    "lang": {\n        "browse": "Setting"\n    },\n    "unit":{\n        "czr":"CZR"\n    },\n    "model_header":{\n        "home":"首页",\n        "transfer":"转账",\n        "contacts":"联系人",\n        "setting":"设置",\n        "total":"合计"\n    },\n    "page_setting":{\n        "tit":"设置"\n    }\n}】'
                     */
                    for (let i = 0; i < result.raw.sentences.length; i++) {
                        // console.log(result.raw.sentences[i].orig == p2, , match, p2)
                        let sourcesStr = result.raw.sentences[i].orig.replace("\n", '');
                        let targetStr = result.raw.sentences[i].trans.replace("\n", '');
                        if (sourcesStr == p2) {
                            return ':"' + targetStr + '"'
                        }

                    }
                    return match
                })

                file.contents = new Buffer(content);
                // Make sure the file goes to the next gulp plugin 
                this.push(file);
                // Tell the stream engine that we have finished processing this file
                cb();

            })
        }
    });
    // return stream file
    return stream;
}

/**
 * Get data to translate
 * @param src
 * @param dst
 * @returns {*}
 */
function getText(src, dst) {
    for (let k in src) {
        if (src[k] instanceof Object) {
            dst = getText(src[k], dst);
        }
        else {
            dst += src[k] + "\n";
        }
    }
    return dst;
}
module.exports = i18n;
```

## languges_conf.js文件如下

`languges_conf.js`

```javascript
const languges = {

    'af': "Burry (Afrikaans)",//布尔语(南非荷兰语)       Burry (Afrikaans)
    'sq': "shqiptar",//阿尔巴尼亚语                     shqiptar
    'am': "አማርኛ",//阿姆哈拉语				           አማርኛ
    'ar': "عربي",//阿拉伯语				                عربي    
    'hy': "Հայերեն",//亚美尼亚语				        Հայերեն
    'az': "Azərbaycan",//阿塞拜疆语				        Azərbaycan
    'eu': "Euskal",//巴斯克语				            Euskal
    'be': "беларускі",//白俄罗斯语				        беларускі
    'bn': "বাংলা ভাষার",//孟加拉语				            বাংলা ভাষার
    'bs': "Bosanski",//波斯尼亚语				        Bosanski

    'bg': "български",//保加利亚语				        български
    'ca': "Català",//加泰罗尼亚语			            Català
    'ceb': "Sugbo",//宿务语				                Sugbo
    'zh-CN': "中文(简体)",//中文(简体)
    'zh-TW': "中文(繁體)",//中文(繁體)
    'co': "Corsa",//科西嘉语				    Corsa
    'hr': "Croata",//克罗地亚语				    Croata
    'cs': "Česky",//捷克语				        Česky
    'da': "dansk",//丹麦语				        dansk
    'nl': "Nederlands",//荷兰语				    Nederlands

    'en': "English",//英语					    English
    'eo': "Esperanto",//世界语				    Esperanto
    'et': "Eesti keel",//爱沙尼亚语				 Eesti keel
    'fi': "suomalainen",//芬兰语				suomalainen
    'fr': "Français",//法国语				    Français
    'fy': "Frysk",//弗里西语				    Frysk
    'gl': "Galego",//加利西亚语				    Galego
    'ka': "ქართული",//格鲁吉亚语				ქართული
    'de': "Deutsche Sprache",//德语			    Deutsche Sprache
    'el': "Ελληνικά",//希腊语				    Ελληνικά

    'gu': "ગુજરાતી",//古吉拉特语				    ગુજરાતી
    'ht': "Kreyòl Ayisyen",//海地克里奥尔语			Kreyòl Ayisyen
    'ha': "Hausa",//豪萨语				        Hausa
    'haw': "Hawaiian",//夏威夷语				Hawaiian
    'iw': "עברית",//希伯来语				    עברית
    'hi': "बात मत करो",//印地语				    बात मत करो
    'hmn': "Miao",//苗语					    Miao
    'hu': "magyar",//匈牙利语				    magyar
    'is': "Íslensku",//冰岛语				    Íslensku
    'ig': "Asụsụ Ibo",//伊博语				    Asụsụ Ibo

    'id': "Bahasa indonesia",//印尼语	    Bahasa indonesia
    'ga': "Gaeilge",//爱尔兰语				Gaeilge
    'it': "lingua italiana",//意大利语		lingua italiana
    'ja': "日本語",//日语				    日本語
    'jw': "Wong Jawa",//印尼爪哇语			Wong Jawa
    'kn': "ಕನ್ನಡ",//卡纳达语				    ಕನ್ನಡ
    'kk': "Қазақша",//哈萨克语				Қазақша
    'km': "ភាសាខ្មែរ",//高棉语				    ភាសាខ្មែរ
    'ko': "한국어",//韩语					    한국어
    'ku': "Kurdî",//库尔德语				    Kurdî

    'ky': "Кыргыз тили",//吉尔吉斯语		Кыргыз тили
    'lo': "ລາວ",//老挝语				    ລາວ
    'la': "Latine",//拉丁语				    Latine
    'lv': "Latviešu",//拉脱维亚语				Latviešu
    // 'lt': "Lietuviškai",//立陶宛语				Lietuviškai
    'lb': "Lëtzebuergesch",//卢森堡语			Lëtzebuergesch
    'mk': "Македонски",//马其顿语				Македонски
    'mg': "Malagasy",//马尔加什语				Malagasy
    'ms': "Melayu",//马来语				    Melayu
    'ml': "മലയാളം",//马拉雅拉姆语			മലയാളം

    'mt': "Malti",//马耳他语				Malti
    'mi': "Maori",//毛利语				    Maori
    'mr': "मराठी",//马拉地语				    मराठी
    'mn': "Монгол хэл",//蒙古语				Монгол хэл
    'my': "မြန်မာ",//缅甸语				    မြန်မာ
    'ne': "नेपाली",//尼泊尔语				नेपाली
    'no': "norsk språk",//挪威语			norsk språk
    'ny': "Chichewa",//齐切瓦语				Chichewa
    'ps': "پښتو",//普什图语				    پښتو
    'fa': "فارسی",//波斯语				    فارسی

    'pl': "Polski",//波兰语				    Polski
    'pt': "Português",//葡萄牙语			Português
    'pa': "ਪੰਜਾਬੀ",//旁遮普语				   ਪੰਜਾਬੀ
    'ro': "românesc",//罗马尼亚语			românesc
    'ru': "Русский язык",//俄语				Русский язык
    'sm': "Samoa",//萨摩亚语				Samoa
    'gd': "Gàidhlig na h-Alba",//苏格兰盖尔语			Gàidhlig na h-Alba
    'sr': "Српски",//塞尔维亚语				    Српски
    'st': "Sesotho",//塞索托语				    Sesotho
    'sn': "Shinra",//修纳语				    Shinra

    'sd': "سنڌي",//信德语				    سنڌي
    'si': "සිංහල",//僧伽罗语				සිංහල
    'sk': "slovenského jazyk",//斯洛伐克语	    slovenského jazyk
    'sl': "Slovenščina",//斯洛文尼亚语			Slovenščina
    'so': "Somali",//索马里语				    Somali
    'es': "Español",//西班牙语				    Español
    'su': "Sunda Indonesian",//印尼巽他语       Sunda Indonesian
    'sw': "Kiswahili",//斯瓦希里语				Kiswahili
    'sv': "Svenska",//瑞典语				    Svenska
    'tl': "Filipino",//菲律宾语				    Filipino

    'tg': "Тоҷикӣ",//塔吉克语				    Тоҷикӣ
    'ta': "தமிழ் மொழி",//泰米尔语				    தமிழ் மொழி
    'te': "తెలుగు",//泰卢固语				    తెలుగు
    'th': "ไทย",//泰语					        ไทย
    'tr': "Türk dili",//土耳其语				Türk dili
    'uk': "Українська",//乌克兰语				Українська
    'ur': "اردو",//乌尔都语				    اردو
    'uz': "O'zbek",//乌兹别克语				O'zbek
    'vi': "Tiếng Việt",//越南语				Tiếng Việt
    'cy': "Cymraeg",//威尔士语				Cymraeg

    'xh': "IsiXhosa saseMzantsi Afrika",//南非科萨语    IsiXhosa saseMzantsi Afrika
    'yi': "ייִדיש",//意第绪语				            ייִדיש
    'yo': "Yorùbá",//约鲁巴语				            Yorùbá
    'zu': "I-South African Zulu"//祖鲁语                I-South African Zulu
};

module.exports = languges;
```

## tap-i18n.json

```json
{
    "confirm": "确认",
    "cancel": "取消",
    "dialog_tit": "提示",
    "close": "关闭",
    "confirm_you_active": "请确认您的操作",
    "unit": {
        "czr": "CZR"
    },
    "model_header": {
        "home": "首页",
        "transfer": "转账",
        "contacts": "联系人",
        "setting": "设置",
        "total": "合计",
        "testnet":"测试网络"
    },
    "page_setting": {
        "tit": "设置",
        "lang": "语言",
        "select": "请选择"
    },
    "page_contacts": {
        "tit": "联系人",
        "add_dialog": "新建联系人",
        "add_cont": {
            "tit": "新建联系人",
            "tag": "联系人",
            "address": "账户地址",
            "tag_placeholder": "请输入联系人的姓名",
            "address_placeholder": "请输入联系人的账户地址",
            "add_success": "添加成功"
        },
        "delete_dialog": {
            "title": "您正在移除",
            "remove_success": "移除成功"
        },
        "msg_info": {
            "no_tag": "请输入账户备注",
            "no_address": "请输入账户地址",
            "valid_address": "联系人的地址不合法",
            "validate_tag_length": "备注名长度不能大于8个字符",
            "exist": "联系人已经存在,"
        }
    },
    "page_transfer": {
        "from_address": "发款方",
        "to_address": "收款方",
        "amount": "金额",
        "send_all": "发送全部",
        "data": "Data",
        "data_placeholder": "十六进制数据",
        "fees": "手续费",
        "total": "合计",
        "select": "请选择",
        "no_account_info": "暂无账号，请创建或者导入账号",
        "contacts_dig": {
            "title":"选择联系人账号",
            "select_placeholder": "请选择"
        },
        "confirm_dia": {
            "title": "请确认",
            "enter_passworld_tit": "请输入密码",
            "enter_passworld_place": "请输入密码"
        },
        "msg_info": {
            "address_null": "请输入对方账户",
            "balance_zero": "余额不足，无法继续",
            "amount_zero": "转账金额必须大于0",
            "amount_error": "请输入合法的交易金额",
            "address_err": "转入的地址格式不合法",
            "send_success": "发送成功",

            "decrypt_err": "钱包解析失败，可能是密码错误",
            "send_error": "交易失败：请检查是否有足量余额和矿工费"
        }
    },
    "page_account": {
        "transfer_log": "交易记录",
        "transfer_log_null": "暂无交易记录",
        "active_icon": {
            "go_transfer": "发起转账",
            "import_keystore": "账户文件",
            "qrcode": "二维码"
        },
        "edit_dia": {
            "tit": "修改备注",
            "subtit": "为您的账户标记一个备注",
            "no_tag": "请输入账户备注",
            "validate_tag_length": "备注名长度不能大于8个字符"
        },
        "dia_tx":{
            "tit":"交易信息",
            "block_hash":"交易号",
            "status":"状态",
            "from":"发款方",
            "to":"收款方",
            "amount":"金额",
            "data":"数据",
            "send_time":"发送时间",
            "mac_time":"主链时间"
        },
        "keystore": {
            "tit": "备份钱包文件",
            "copy": "复制",
            "download": "下载钱包文件"
        },
        "msg_info": {
            "ads_copy_success": "地址已经复制",
            "key_copy_success": "复制成功"
        }
    },
    "page_home": {
        "import_account": "导入账号",
        "add_account": "新建账号",
        "acc": "账户",
        "create_dia": {
            "create_tit": "创建账户",
            "backup_tit": "备份您的新账户",
            "create_tag": "账户备注",
            "create_pwd": "设置密码",
            "create_repwd": "确认密码",
            "placeholder_tag": "请设置账户备注名",
            "placeholder_pwd": "请设置账户密码",
            "placeholder_repwd": "请确认账户密码",
            
            
            "account_download_keystore": "下载账户文件",

            "validate_tag": "请输入账户备注名",
            "validate_tag_length": "备注名长度不能大于8个字符",

            "validate_password": "请输入您设置的密码",
            "validate_re_password": "请再次输入您设置的密码",
            "validate_strong_password": "请输入一个至少8位的密码",
            "validate_password_length": "两次输入的密码不一致",

            "create_success": "创建成功",
            "create_success_des": "请保存你的账户文件，不要分享给任何人！"
        },
        "import_dia": {
            "tit": "导入账户",    
            "placeholder_keystore": "请将账户文件拖入此处",
            "create_tag": "账户备注",
            "placeholder_tag": "请设置账户备注名",

            "validate_tag": "请输入账户备注名",
            "validate_tag_length": "备注名长度不能大于8个字符",
            "validate_enter_keystore": "请先导入账户文件",
            "validate_error_keystore": "账户导入失败，可能是账户文件错误",
            "imported_account_success": "导入成功",
            "exist": "此账户已经存在,备注是",
            "keystore_error": "错误：",
            "imported_success":"导入文件成功"
        },
        "remove_dia": {
            "tit": "你确定要移除吗？",
            "remove_confrim": "我确定移除",
            "backup": "请注意此账号的备份，避免财产损失",
            "input_pwd":"密码",
            "placeholder_pwd":"请输入此账户的密码",
            "validate_password": "请输入账户的密码",
            "remove_success": "移除成功"
        }
    }
}
```