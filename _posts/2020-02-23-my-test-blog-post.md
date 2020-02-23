---
layout: post
title:  "What is New In PHp 7.4"
author: "Cagan"
comments: true

---



# What is New In PHP 7.4

Probably most impressive thing that makes the new version of the PHP better is speed improvement. 

 

```php
<?php

namespace App\Http\Controllers;

use App\Contracts\CheckerInterface;
use App\Contracts\RequestSenderInterface;
use App\Payloads\TicketPayload;

class TicketChecker extends Controller implements CheckerInterface
{
    public const PAYLOADS = [
        'api/ticket/play' => TicketPayload::PLAY,
        'api/ticket/cancel' => TicketPayload::CANCEL,
        'api/ticket/result' => TicketPayload::RESULT,
        'api/ticket/solve' => TicketPayload::SOLVE,
    ];
    protected static $failed = [];
    protected static $success = [];
    protected $baseUrl;

    public function __construct(string $baseUrl)
    {
        if (substr($baseUrl, -1) !== '/') {
            $baseUrl .= '/';
        }

        $this->baseUrl = $baseUrl;
    }

    public function check(RequestSenderInterface $sender)
    {
        foreach (self::PAYLOADS as $URL => $PAYLOAD) {
            $response = $sender->send("{$this->baseUrl}{$URL}", $PAYLOAD, true);

            if ($response['response_data']->getStatusCode() === 200) {
                ob_start();
                dump($response['request_data']);
                $requestHtml = ob_get_clean();

                ob_start();
                dump($response);
                $responseHtml = ob_get_clean();

                static::$success[] = json_encode(
                    [
                        'request' => $response['request_data'],
                        'response' => [
                            'statusCode' => $response['response_data']->getStatusCode(),
                            'headers' => $response['response_data']->getHeaders(),
                            'body' => $response['response_data']->getBody()->getContents(),
                        ],
                        'url' => $URL,
                        'request_html' => $requestHtml,
                        'response_html' => $responseHtml,
                    ],
                    JSON_PRETTY_PRINT
                );
            } else {
                ob_start();
                dump($response['request_data']);
                $requestHtml = ob_get_clean();

                ob_start();
                dump($response);
                $responseHtml = ob_get_clean();

                static::$failed[] = json_encode(
                    [
                        'request' => $response['request_data'],
                        'response' => [
                            'statusCode' => $response['response_data']->getStatusCode(),
                            'headers' => $response['response_data']->getHeaders(),
                            'body' => json_encode($response['response_data']->getBody()->getContents()),
                        ],
                        'url' => $URL,
                        'request_html' => $requestHtml,
                        'response_html' => $responseHtml,
                    ],
                    JSON_PRETTY_PRINT
                );
            }
        }

        return [
            'success' => static::$success,
            'failed' => static::$failed,
        ];
    }
}

```

