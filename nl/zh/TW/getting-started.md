---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# 開始使用 {{site.data.keyword.blockstorageshort}}

簡要說明小節應該包括一到兩個句子，用以說明系統管理者或 DevOps 工程師為何要使用此基礎架構供應項目或服務。簡單提及使用者的學習目標，並在標題及（或）簡要說明中包括下列 SEO 關鍵字：IBM Cloud、ServiceName。請務必使用交談樣式。如需詳細資料，請參閱 Carbon Design System 中的交談樣式指引，網址為 http://www.carbondesignsystem.com/guidelines/content/general。

範例：資源需求驟增時，您需要可以在控制下橫向擴充以立即符合這些新需求的雲端基礎架構服務。在適合您工作負載的地理區域中，只要幾分鐘，就能透過您選擇的虛擬伺服器映像檔來部署「{{site.data.keyword.Bluemix}} 虛擬伺服器」。只要工作負載消失，就可以暫停或關閉那些虛擬伺服器的電源，讓您的雲端環境完美地符合基礎架構需求。

作業區段包括裝置、儲存空間或網路的設定及執行步驟。
- 使用以作業為基礎的技術資訊，減少交談樣式，以支持簡明扼要且直接的指示。
- 包括基本最常用的情境步驟，以使用基礎架構服務。
- 不要包括從 Bluemix 型錄新增服務的步驟；我們假設使用者已在使用者介面中執行新增服務的步驟。
- 包括以所有語言撰寫且可複製的程式碼 Snippet，以及 VCAP 服務資訊。VCAP 服務資訊的相關資訊，請參閱 https://console.ng.bluemix.net/docs/cli/vcapsvc.html
- 針對配置、管理等等其他作業，請在作業區段或所使用的「關於」區段下面新增作業區段 (## Gerund_task_title)。使用 "Configuring x"、"Administering y"、"Managing z" 這類作業標題。-->

## 必要條件
管理者必須先具有已升級的 {{site.data.keyword.Bluemix}} 帳戶，才能佈建或管理其基礎架構供應項目。如需相關資訊，請參閱[升級與統一 {{site.data.keyword.Bluemix_notm}} 及 SoftLayer 計費帳戶](../docs/admin/softlayerlink.html)。

## 作業導向的標題及說明
若要快速開始進行此基礎架構服務，請遵循以下步驟：-或者-
完成以下步驟，以開始使用 Block Storage 服務：

<!-- Use ordered list markup for the step section. For code examples:
- use three backticks ahead of and after the example (```)
- For copyable code snippet, multi-line, include {: codeblock} following the last set of backticks. A copy button will display in framework in output.
- For copyable command, single line, include {: pre} following the last set of backticks. When displayed, it will show "$" at the beginning of the command example and a copy button, but the copy button will include just the command example.
- For non-copyable output snippet, include {: screen} following the last set of backticks.
 -->

1. 步驟 1 設定服務。
2. 步驟 2 設定服務。

	```
	Copyable example for this step.
	This example might be multiline code
	to copy into a file.
	When displayed in the doc framework,
	it will have a copy button on the right.
	The user can click to copy the example
	so they can paste it into their code editor.
	```
	{: codeblock}

3. 步驟 3. 在此步驟中，我們有一個單行的指令範例。由文件架構顯示時，會在指令行開頭顯示 $，並在右側顯示複製按鈕。複製按鈕將會複製指令，但不複製 $。

	```
	my command -and -options
	```
	{: pre}

4. 步驟 4
	```
	This is a bunch of output from
		a command or program I ran
			and it can run lots of lines
			and the doc framework will show it as
			output with no copy button.
	```
	{: screen}

## 後續步驟

