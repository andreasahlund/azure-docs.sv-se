---
title: Använda tal-SDK för uttal av uttal
titleSuffix: Azure Cognitive Services
description: 'Tal-SDK: n stöder uttal av uttal, som utvärderar uttal av tal ingångar, med indikatorer på precision, Fluency, fullständighet osv.'
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/29/2020
ms.author: yulili
ms.custom: references_regions
zone_pivot_groups: programming-languages-set-nineteen
ms.openlocfilehash: 1cc313daf0e76ddd14865959410b07d9bdc189d7
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/20/2020
ms.locfileid: "94984929"
---
# <a name="pronunciation-assessment"></a>Uttal av uttal

Uttal-utvärderingen utvärderar tal uttal och ger talare feedback om precisionen och Fluency av talade ljud.
Med uttals-utvärdering kan språklärare öva, få snabb feedback och förbättra sina uttal så att de kan tala och presentera med förtroende.
Lärare kan använda funktionen för att utvärdera uttal av flera högtalare i real tid.

I den här artikeln lär du dig att konfigurera `PronunciationAssessmentConfig` och hämta `PronunciationAssessmentResult` med hjälp av talet SDK.

> [!NOTE]
> Funktionen för ututtals bedömning är för närvarande bara tillgänglig i regioner `westus` , och `eastasia` `centralindia` stöder bara språk `en-US` .

## <a name="pronunciation-assessment-with-the-speech-sdk"></a>Uttal av tal-SDK

I exemplen nedan skapar du en `PronunciationAssessmentConfig` och tillämpar den på en `SpeechRecognizer` .

Följande kodfragment visar hur du använder automatisk språk identifiering i dina appar:

::: zone pivot="programming-language-csharp"

```csharp
var pronunciationAssessmentConfig = new PronunciationAssessmentConfig(
    "reference text", GradingSystem.HundredMark, Granularity.Phoneme);

using (var recognizer = new SpeechRecognizer(
    speechConfig,
    audioConfig))
{
    // apply the pronunciation assessment configuration to the speech recognizer
    pronunciationAssessmentConfig.ApplyTo(recognizer);
    var speechRecognitionResult = await recognizer.RecognizeOnceAsync();
    var pronunciationAssessmentResult =
        PronunciationAssessmentResult.FromResult(speechRecognitionResult);
    var pronunciationScore = pronunciationAssessmentResult.PronunciationScore;
}
```

::: zone-end

::: zone pivot="programming-language-cpp"

```cpp
auto pronunciationAssessmentConfig =
    PronunciationAssessmentConfig::Create("reference text",
        PronunciationAssessmentGradingSystem::HundredMark,
        PronunciationAssessmentGranularity::Phoneme);

auto recognizer = SpeechRecognizer::FromConfig(
    speechConfig,
    audioConfig);

// apply the pronunciation assessment configuration to the speech recognizer
pronunciationAssessmentConfig->ApplyTo(recognizer);
speechRecognitionResult = recognizer->RecognizeOnceAsync().get();
auto pronunciationAssessmentResult =
    PronunciationAssessmentResult::FromResult(speechRecognitionResult);
auto pronunciationScore = pronunciationAssessmentResult->PronunciationScore;
```

::: zone-end

::: zone pivot="programming-language-java"

```java
PronunciationAssessmentConfig pronunciationAssessmentConfig =
    new PronunciationAssessmentConfig("reference text", 
        PronunciationAssessmentGradingSystem.HundredMark,
        PronunciationAssessmentGranularity.Phoneme);

SpeechRecognizer recognizer = new SpeechRecognizer(
    speechConfig,
    audioConfig);

// apply the pronunciation assessment configuration to the speech recognizer
pronunciationAssessmentConfig.applyTo(recognizer);
Future<SpeechRecognitionResult> future = recognizer.recognizeOnceAsync();
SpeechRecognitionResult result = future.get(30, TimeUnit.SECONDS);
PronunciationAssessmentResult pronunciationAssessmentResult =
    PronunciationAssessmentResult.fromResult(result);
Double pronunciationScore = pronunciationAssessmentResult.getPronunciationScore();

recognizer.close();
speechConfig.close();
audioConfig.close();
pronunciationAssessmentConfig.close();
result.close();
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
pronunciation_assessment_config = \
        speechsdk.PronunciationAssessmentConfig(reference_text='reference text',
                grading_system=msspeech.PronunciationAssessmentGradingSystem.HundredMark,
                granularity=msspeech.PronunciationAssessmentGranularity.Phoneme)
speech_recognizer = speechsdk.SpeechRecognizer(
        speech_config=speech_config, \
        audio_config=audio_config)

# apply the pronunciation assessment configuration to the speech recognizer
pronunciation_assessment_config.apply_to(speech_recognizer)
result = speech_recognizer.recognize_once()
pronunciation_assessment_result = speechsdk.PronunciationAssessmentResult(result)
pronunciation_score = pronunciation_assessment_result.pronunciation_score
```

::: zone-end

::: zone pivot="programming-language-objectivec"

```Objective-C
SPXPronunciationAssessmentConfiguration* pronunciationAssessmentConfig =
    [[SPXPronunciationAssessmentConfiguration alloc]init:@"reference text"
                                           gradingSystem:SPXPronunciationAssessmentGradingSystem_HundredMark
                                             granularity:SPXPronunciationAssessmentGranularity_Phoneme];

SPXSpeechRecognizer* speechRecognizer = \
        [[SPXSpeechRecognizer alloc] initWithSpeechConfiguration:speechConfig
                                              audioConfiguration:audioConfig];

// apply the pronunciation assessment configuration to the speech recognizer
[pronunciationAssessmentConfig applyToRecognizer:speechRecognizer];

SPXSpeechRecognitionResult *result = [speechRecognizer recognizeOnce];
SPXPronunciationAssessmentResult* pronunciationAssessmentResult = [[SPXPronunciationAssessmentResult alloc] init:result];
double pronunciationScore = pronunciationAssessmentResult.pronunciationScore;
```

::: zone-end

### <a name="pronunciation-assessment-configuration-parameters"></a>Konfigurations parametrar för uttal-bedömning

Den här tabellen innehåller konfigurations parametrar för uttal-utvärdering.

| Parameter | Beskrivning | Obligatoriskt? |
|-----------|-------------|---------------------|
| ReferenceText | Texten som uttalet kommer att utvärderas mot. | Obligatorisk |
| GradingSystem | Punkt systemet för resultat kalibrering. `FivePoint`Systemet ger ett flytt ALS värde på 0-5 och `HundredMark` ger en 0-100 flytt ALS poäng. Standard: `FivePoint`. | Valfritt |
| Precision | Utvärderings precisionen. Godkända värden är `Phoneme` , som visar poängen på den fullständiga text-, Word-och fonem-nivån, `Word` som visar poängen på den fullständiga text-och ord nivån, `FullText` som bara visar poängen på hela text nivån. Standard: `Phoneme`. | Valfritt |
| EnableMiscue | Aktiverar miscue-beräkning. Med det här alternativet kommer uttalade ord att jämföras med referens texten och markeras med utelämnanden/infogning baserat på jämförelsen. Godkända värden är `False` och `True` . Standard: `False`. | Valfritt |
| ScenarioId | Ett GUID som anger ett anpassat punkt system. | Valfritt |

### <a name="pronunciation-assessment-result-parameters"></a>Resultat parametrar för uttal-bedömning

I den här tabellen visas resultat parametrar för uttal-utvärdering.

| Parameter | Beskrivning |
|-----------|-------------|
| `AccuracyScore` | Talts uttal. Noggrannhet anger hur nära fonem som matchar en ursprunglig talares uttal. Precisions poängen för ord och full text nivå sammanställs från precisions poängen på fonem nivå. |
| `FluencyScore` | Fluency för det tal som angetts. Fluency anger hur tätt talet ska matcha en inbyggd högtalares användning av tyst raster mellan ord. |
| `CompletenessScore` | Talet är klart och bestäms genom att beräkna förhållandet mellan uttalade ord och referenser till text ingångar. |
| `PronunciationScore` | Totalt antal poäng som anger uttal av det tal som anges. Detta sammanställs från `AccuracyScore` `FluencyScore` och `CompletenessScore` med vikt. |
| `ErrorType` | Det här värdet anger om ett ord utelämnas, infogas eller blir dåligt uttalad jämfört med `ReferenceText` . Möjliga värden är `None` (vilket innebär inget fel på det här ordet), `Omission` `Insertion` och `Mispronunciation` . |

## <a name="next-steps"></a>Nästa steg

<!-- TODO: update the sample links after release -->

<!-- ::: zone pivot="programming-language-csharp"
* See the [sample code](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#L741) on GitHub for automatic language detection
::: zone-end

::: zone pivot="programming-language-cpp"
* See the [sample code](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#L507) on GitHub for automatic language detection
::: zone-end

::: zone pivot="programming-language-java"
* See the [sample code](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#L521) on GitHub for automatic language detection
::: zone-end

::: zone pivot="programming-language-python"
* See the [sample code](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/python/console/speech_synthesis_sample.py#L434) on GitHub for automatic language detection
::: zone-end

::: zone pivot="programming-language-objectivec"
* See the [sample code](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/objective-c/ios/speech-samples/speech-samples/ViewController.m#L494) on GitHub for automatic language detection
::: zone-end -->

* [Dokumentation om tal SDK-referens](speech-sdk.md)
