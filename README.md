# 关于
PHP-page是一个轻量级的分页类


# 需求
对低版本做了向下支持，但建议使用 PHP 5.3 +

# 安装
```shell
composer require chenbool/curl
```
```php
use HyacinthTechnology\ShopifyClient;
```


# 示例
```php
<?php
    /*
     * shopify创建webhoot
     *
     * 
     */
	function shopify_webhoot(){
        require './src/ShopifyClient.php';
		$shop=""; //店铺地址
		$token=""; //授权安装应用后，返回的token
		$api_key="f93d7ab169cb82fc8cecd3c2654d60e2";
        $secret="1db4f3c727ec3f02532a5bca752d767d";

        $shopifyClient = new ShopifyClient($shop, $token, $api_key, $secret);
		/*
         * 创建一个数组 卸载店铺通知
         * 
         * $params 自动转换成json发送给shopify
         *
         */
        $params=array("webhook"=>array("topic"=>"app/uninstalled","address"=>"https://app.xxx.com/resetapi","format"=>"json"));
        
        //创建webhoot
        $WEBHOOK = $shopifyClient->call("POST", "/admin/api/2019-07/webhooks.json",$params);
        dump($WEBHOOK);
		echo '</br>';
		
		/*1.更多方法请去查看shopify查看
		  https://help.shopify.com/en/api/reference/events/webhook  
		*/
		$params=array(
            "webhook"=>array(
                "topic"=>"app/uninstalled",
                "address"=>"https://app.xxx.com/resetapi", //创建的webhoot请求地址
                "format"=>"json" //json数据
            )
        );
        /*2.
         * $shopifyClient->call(
         * "请求类型",
         * "请求地址/更多地址请去shopify查看",
         * "数据/json" );
         */
    }
    //调用方法
	 shopify_webhoot();
	 
    /*
    * 卸载应用的回调地址
    */
    function resetapi(){
        $argvs = $_POST;
        $shopify_pull_data = json_encode($argvs);
        file_put_contents("shopify_uninstall_app.txt",$shopify_pull_data,FILE_APPEND);
    }
```
