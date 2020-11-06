CREAT
-----

-  ng g lib shtsk-translate
-  cd projects/shtsk-translate/src/lib


CODING
------
- this lib only have service and pipe

    ::
        
        rm shtsk-translate.component.spec.ts
        rm shtsk-translate.component.ts
        rm shtsk-translate.service.spec.ts
        ng g pipe shtsk-translate
        rm shtsk-translate.pipe.spec.ts
        

MODULE
-------

    ::
    
        lib# cat shtsk-translate.module.ts
        import { NgModule,ModuleWithProviders } from '@angular/core';
        import { CommonModule } from '@angular/common';
        import { ShtskTranslatePipe } from './shtsk-translate.pipe';
        import { ShtskTranslateService} from './shtsk-translate.service'
        
        
        @NgModule({
            declarations: [ShtskTranslatePipe],
            imports: [
                CommonModule
            ],
            exports: [ShtskTranslatePipe]
            providers:[ShtskTranslateService]
        })
        export class ShtskTranslateModule {
          public static forRoot(): ModuleWithProviders<ShtskTranslateModule> {
            return {
              ngModule: ShtskTranslateModule,
              providers: [],
            }
          }
        
          public static forChild(): ModuleWithProviders<ShtskTranslateModule> {
            return {
              ngModule: ShtskTranslateModule,
              providers: [],
            }
          }
        }
        

SERVICE
-------

    ::
        
        import ch from "./_data-srv/ch";
        import en from "./_data-srv/ch";
        const SUPPORTED_LANGS = { ch, en };
        
        
        import { Injectable } from '@angular/core';
        
        @Injectable(/*{providedIn: 'root'}*/)
        export class ShtskTranslateService {
          private _lang: string='ch';
          constructor() {}
          public get lang() {
            return this._lang;
          }
          public use(lang: string): void {
            this._lang = lang;
          }
          public translate(cate: string, key: string): string {
            cate = cate === undefined ? "ch" : cate;
            let rslt = SUPPORTED_LANGS[this._lang][cate][key];
            return rslt;
          }
        }


PIPE
----
    
    ::
        
        import { ShtskTranslateService} from './shtsk-translate.service'
        import { Pipe, PipeTransform } from '@angular/core';
        
        @Pipe({
          name: 'translate'
        })
        export class ShtskTranslatePipe implements PipeTransform {
        
          constructor(private _translate_srv: ShtskTranslateService) {}
          transform(value: any, cate?: any): any {
            return this._translate_srv.translate(cate, value);
          }
        }


PUBLIC-API
----------

- cd ../
- vim public-api.ts

    ::
        
        export * from './lib/shtsk-translate.module';
        export * from './lib/shtsk-translate.pipe';


PUBLISH
-------
- ng build shtsk-translate
- npm publish

USAGE
-----





