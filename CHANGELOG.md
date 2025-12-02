# Changelog

## [1.3.0](https://github.com/dnpm-dip/deployment/compare/v1.2.1...v1.3.0) (2025-12-02)


### Features

* Added configurability of image tag for mysql, authup and nginx-proxy docker images via ENV variable ([1199844](https://github.com/dnpm-dip/deployment/commit/119984428f94968c90387120cbde94c882c8c473))


### Bug Fixes

* Adapted polling-config to work out-of-the-box by forwarding to the built-in rev. proxy instead of the backend directly (because of URI-prefix-removal problem) ([05dd705](https://github.com/dnpm-dip/deployment/commit/05dd70555f1aa63b5fb8348a20d7c1e3b9148689))
* Removed trailing whitespace in polling-config, which lead to errors in request handling in polling-module ([f9d431f](https://github.com/dnpm-dip/deployment/commit/f9d431f1618e9795207cd11602a068fa8877eb8c))

## [1.2.1](https://github.com/dnpm-dip/deployment/compare/v1.2.0...v1.2.1) (2025-08-12)


### Bug Fixes

* Added ENV variable TZ for the timezone within the backend, in order for Local(Date)Time to have the correct value instead of UTC time ([931087f](https://github.com/dnpm-dip/deployment/commit/931087fc3fcbacbc860e5dbcfdb8493ec43da559))
* Corrected entry to ENV variables ([a4ae940](https://github.com/dnpm-dip/deployment/commit/a4ae94072dd0172a1521555fd957e4120997b0cc))
* Updated backend docker image name ([9e86c35](https://github.com/dnpm-dip/deployment/commit/9e86c3582d0b2331ed772c0d8307275baa91bfc1))
* Updated list of configurable ENV variables ([f5624ce](https://github.com/dnpm-dip/deployment/commit/f5624ce36204a84107db75338e38234c98fa3987))

## [1.2.0](https://github.com/dnpm-dip/deployment/compare/v1.1.0...v1.2.0) (2025-03-07)


### Features

* add authup robot configuration ([f2fa67b](https://github.com/dnpm-dip/deployment/commit/f2fa67b335da8c869910edefded13c318ec596af))
* add gitignore file to nginx sites-enabled ([4c63ac3](https://github.com/dnpm-dip/deployment/commit/4c63ac3431b7ca12d6cbb1875b4ad9f90bacd138))
* add image tag for frontend and backend ([23dcceb](https://github.com/dnpm-dip/deployment/commit/23dcceb3d77ff121a2f877f3ea184a436e8d913e))
* add mysql as database service ([6760105](https://github.com/dnpm-dip/deployment/commit/67601059ece6545632f5d090f14917d5f4440074))
* add nginx service as proxy for docker network ([5cf38b6](https://github.com/dnpm-dip/deployment/commit/5cf38b68288cf2c71e400378ebd58a0b5c462cad))
* added check for existence of non-template files in init ([ced8845](https://github.com/dnpm-dip/deployment/commit/ced8845a657394342085281d36c75960b9b1fb7a))
* Added config and docker compose service to include HttpPollingModule in the deployment ([9831c67](https://github.com/dnpm-dip/deployment/commit/9831c676b40c41d55fda7219f2dbe2ef213d56e6))
* allways reset robot secret ([c7fe51a](https://github.com/dnpm-dip/deployment/commit/c7fe51a9e4c47a175cf3780220384337390bef12))
* change image url for portal image ([73ac8af](https://github.com/dnpm-dip/deployment/commit/73ac8afe79aec2cb0f6673e0ac30d80c2a8b8fcb))
* Changed Backend service persistence to also use Docker Volume instead of directory; fix: Fixed error in 'reverse-proxy.template.conf' ([41b6f4a](https://github.com/dnpm-dip/deployment/commit/41b6f4aab756f15b3657cbc2420e097450f24937))
* create cert template files ([835fdd9](https://github.com/dnpm-dip/deployment/commit/835fdd9de6e1e876b20d7b356086262c5001d73c))
* create default cert folder ([41957d7](https://github.com/dnpm-dip/deployment/commit/41957d763e84f5b3cdc7a96f3568f44de6c9b45c))
* create template files for .env & backend-config files ([f49390a](https://github.com/dnpm-dip/deployment/commit/f49390a0c73276833906a223909e96f6a1e81b80))
* extended environment variables ([81dce77](https://github.com/dnpm-dip/deployment/commit/81dce775fbf1388bb5fbde1ddc0078796cef3844))
* Final adaptations to template files and README ([bc6e960](https://github.com/dnpm-dip/deployment/commit/bc6e960ecfc80505f34028ffd60e1ad97fbe1d24))
* highligh text block in detailed set-up ([80db951](https://github.com/dnpm-dip/deployment/commit/80db9519a82600dfa36b01bba6a7b6fdf832f1af))
* implemenet release workflow ([a1da6f8](https://github.com/dnpm-dip/deployment/commit/a1da6f8eb75e3da4ceff29dd268f55456dd728fa))
* initialized repository ([c29282c](https://github.com/dnpm-dip/deployment/commit/c29282cd9ac554625e98869854734bd5e3c45f2d))
* Made download URL for the HGNC GeneSet JSON configurable via ENV variable ([7fa086c](https://github.com/dnpm-dip/deployment/commit/7fa086c79434b19aa0731a5bb0c4aaee103e47fa))
* make docker image addresses configurable ([cedfb09](https://github.com/dnpm-dip/deployment/commit/cedfb09d80b9b75f2b82fe17fb5a242c0fd4dac0))
* make subnet editable via env variable ([73561c1](https://github.com/dnpm-dip/deployment/commit/73561c1c387a9c89a117de40690cf67a12591b95))
* merge nginx configurations ([6175eee](https://github.com/dnpm-dip/deployment/commit/6175eee0bdc979589244ddbb99d23016464775da))
* persistent volume for mysql + fix volume mounts for nginx ([d89973c](https://github.com/dnpm-dip/deployment/commit/d89973ca7968a8dcc0c56ce5330d2e63a389f589))
* Replaced obsolete certificate chain of the central NGINX Broker server certificate, and added migration note ([ce95a57](https://github.com/dnpm-dip/deployment/commit/ce95a5785e29cda18614b3ff992ac325ed7813f5))
* route traffic on server side rendering internally ([da3f5d6](https://github.com/dnpm-dip/deployment/commit/da3f5d6a7a51b0a4fe6a026a0cb4f6a6ce7eb526))
* use variable for upstream ([e13f611](https://github.com/dnpm-dip/deployment/commit/e13f611b73874d70d205a7c419902126902dc00c))
* use variables in docker-compose file ([a877fc2](https://github.com/dnpm-dip/deployment/commit/a877fc2327c9d490071261bca56165d2b8d297eb))


### Bug Fixes

* Adapted README with polling setup instructions ([0ae8a47](https://github.com/dnpm-dip/deployment/commit/0ae8a47b837c951866d35c9c747aac2f7502d19c))
* add depends_on directive for backend container ([36c8db3](https://github.com/dnpm-dip/deployment/commit/36c8db3dd98d7e49198c38b10731e9a3299e790c))
* add depends_on for authup service ([490159a](https://github.com/dnpm-dip/deployment/commit/490159ac9ff600585774cb660c2bcf4bd3cccad0))
* add missing authup url via env variable ([610ca56](https://github.com/dnpm-dip/deployment/commit/610ca565209de55291ce8f80bc23e8d0883e3e82))
* Added alternative location block to forward-proxy.conf and adapted README ([bd2649b](https://github.com/dnpm-dip/deployment/commit/bd2649b30a403f47e8afab661bade54b97f6bd81))
* Added migration note to README ([50326f2](https://github.com/dnpm-dip/deployment/commit/50326f2813e42511c7de539a97f019385ed0e627))
* Added note to clarify when DNPM client certificate is required ([0b442c4](https://github.com/dnpm-dip/deployment/commit/0b442c4c6d29a2732e2e0acbfabfd2c396de3235))
* Added notes about HGNC gene set provision ([77d8f3e](https://github.com/dnpm-dip/deployment/commit/77d8f3ed1daaa16639cc4a700fc0ce1fba437885))
* Added rewrite rule to NGINX config templates ([8ac8ddf](https://github.com/dnpm-dip/deployment/commit/8ac8ddf0caaf9d826994835530a4efc4b3fa8905))
* adjust paths of certs in docker-compose.yml ([dbe0bbf](https://github.com/dnpm-dip/deployment/commit/dbe0bbfcd3590dce39883a50a8ced9de28b2846c))
* adjusted nginx sites-available templates ([94a0f78](https://github.com/dnpm-dip/deployment/commit/94a0f7851337957c72dfed0c77139521382f7253))
* bridge name ([6f34336](https://github.com/dnpm-dip/deployment/commit/6f343360626381ada2627f55c3457b70664f7b80))
* Committed correctly renamed certificate files ([a79b585](https://github.com/dnpm-dip/deployment/commit/a79b5856173e4a061357c2112b8b3947c084e464))
* Corrected link to multi-line block ([bd2336e](https://github.com/dnpm-dip/deployment/commit/bd2336ec0e1c67908e15824ad1d6050a95b6e348))
* Corrected typo ([ba19983](https://github.com/dnpm-dip/deployment/commit/ba19983ed3124ca2248d8f6bb73cecfc60446c80))
* Corrected typo ([48d8f26](https://github.com/dnpm-dip/deployment/commit/48d8f265a8e78bf92c2899fd335a9261bd124fee))
* Corrected typos ([615e64e](https://github.com/dnpm-dip/deployment/commit/615e64e7ea33d79cb29331bd19496c84484069d8))
* Corrections/improvements to README ([8c01a54](https://github.com/dnpm-dip/deployment/commit/8c01a5450ca98b0fabcb062d8909aa2b488fb435))
* Couple of additions to README ([08bcb8d](https://github.com/dnpm-dip/deployment/commit/08bcb8d9d3abfef4cb03f3780f4eaf5ae1704ecc))
* Couple of corrections/clarifications in README ([e5e4a12](https://github.com/dnpm-dip/deployment/commit/e5e4a122dfd78bceb37a3a864246f14392fde766))
* Disabled 'allowed hosts filter' in production.conf template, and adapted README accordingly ([677e62b](https://github.com/dnpm-dip/deployment/commit/677e62b93d9b0f5f27d55ac44bc7ebb37e85ccaa))
* Fixed erroneous proxying directive in forward proxy config ([ab9dca2](https://github.com/dnpm-dip/deployment/commit/ab9dca200c02c0889f41b62d307a522a48a22b49))
* Fixed merge conflict ([6cdec73](https://github.com/dnpm-dip/deployment/commit/6cdec731e9c6c50cf02539ccce75088ae5b42d87))
* Little improvements to current migration note in README ([23e29a5](https://github.com/dnpm-dip/deployment/commit/23e29a590dafa8a298a8fe1a52472d25b7426c00))
* make network name configurable ([7780285](https://github.com/dnpm-dip/deployment/commit/7780285e8f610cea6e9b7daeae8a09c1a67d8250))
* merge markdown tables + removed unnecessary env variables ([322f661](https://github.com/dnpm-dip/deployment/commit/322f6615d159198f8ebfcbc711df10a02cefbdb9))
* Minor additions/corrections to README ([4cf4aa6](https://github.com/dnpm-dip/deployment/commit/4cf4aa659b27e1f05e88a809c7537476fbf86c38))
* Minor change to README ([09fcb83](https://github.com/dnpm-dip/deployment/commit/09fcb83ec02e910435be6d799ecb7a6c3de0618e))
* Minor corection to README ([dbe5883](https://github.com/dnpm-dip/deployment/commit/dbe5883dc0034ec3483363e87f1da334ef88ac75))
* Minor correction ([40eea2b](https://github.com/dnpm-dip/deployment/commit/40eea2b0682b58648497c4c81213c75143a616bb))
* Minor corrections to README ([1472409](https://github.com/dnpm-dip/deployment/commit/1472409ffffff830c5bb7232885a46e1de993a49))
* Minor editing in README ([305687d](https://github.com/dnpm-dip/deployment/commit/305687dea8f676dd21035000b6749176db6acc55))
* Minor layouting for clarity ([477c31e](https://github.com/dnpm-dip/deployment/commit/477c31e928b8e6cb2e6003702e1dc5a0d668607b))
* minor typo in network bridge name ([c3a2e7e](https://github.com/dnpm-dip/deployment/commit/c3a2e7e7e3ee7ef7261c44aa8e92b51ade1ee7d3))
* Remote verification of the NGINX broker certificate also works using only the HARICA Root CA certificate, so removed rest of the chain ([c7e5936](https://github.com/dnpm-dip/deployment/commit/c7e593622c5dc0e7fb8b98e0093add4c658f5246))
* remove docker.sock mounting for nginx container ([9dbb272](https://github.com/dnpm-dip/deployment/commit/9dbb272e6fb784b411e4a24aca1bd50da1e7df2d))
* remove explicit container_name definition ([84c3a7a](https://github.com/dnpm-dip/deployment/commit/84c3a7ab81bc778d8ed4883625c51ae9dd8b047f))
* Removed known issue about HGNC gene set download now that it's fixed ([804d792](https://github.com/dnpm-dip/deployment/commit/804d792a11ed812ceb70a4a351d5a055b4122824))
* set correct redirect for oidc login ([#3](https://github.com/dnpm-dip/deployment/issues/3)) ([c822cdd](https://github.com/dnpm-dip/deployment/commit/c822cddb5b02c3c985de0bbd108768b4beb6dd7f))
* set default local volumes of backend ([54adbc7](https://github.com/dnpm-dip/deployment/commit/54adbc7007ee18cc0b4ac03dc5ab6ce0b60eed30))
* set env as ignored file ([dc368c0](https://github.com/dnpm-dip/deployment/commit/dc368c05db84d79ce34607979b357ef968293e71))
* simplify .env template file ([0790246](https://github.com/dnpm-dip/deployment/commit/07902469316e85a15af5c5733531514537e1bc8f))
* test of markdown rendering in README ([bb273e8](https://github.com/dnpm-dip/deployment/commit/bb273e8f64e1d7f954c9066e0f80f532ea3d1032))
* Updated links in HGNC provision notes ([06ea387](https://github.com/dnpm-dip/deployment/commit/06ea3878252a69c6b8a0bbed7081218de6560411))
* updated public url of backend ([14d5236](https://github.com/dnpm-dip/deployment/commit/14d5236f4bb4b9326d861e730bad38780d0b76b5))
* Updated README ([465923f](https://github.com/dnpm-dip/deployment/commit/465923f915cdb971faee47fddad8e185a3b385c3))
* Updated referenced backend image to 'dnpm-dip' organization ([3be39cd](https://github.com/dnpm-dip/deployment/commit/3be39cdf2093e9c7d2260cc957394339e3436433))
* use local directory path for backend ([aa4f34a](https://github.com/dnpm-dip/deployment/commit/aa4f34a1ad0d89b12d89314e8903962de37847b9))
* volume path for authup service ([4b94763](https://github.com/dnpm-dip/deployment/commit/4b947639126ad52c940c8af54a2f9cc1eda42069))


### Reverts

* "fix: remove docker.sock mounting for nginx container" ([77f453f](https://github.com/dnpm-dip/deployment/commit/77f453f1c7e7e42d9e8bc8da56be83e08bfde4b4))
