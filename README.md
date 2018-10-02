# Angular-Code-Style

List of Modules
---------------
	- NgModule (Main Module)
	- BrowserModule
	- BrowserAnimationsModule
	- HttpClientModule
	- RouterModule
	- FormsModule
	- ReactiveFormsModule
	- BrowserTestingModule
	- CommonModule



List import Paths
-----------------
	# Core:
		- import { 
			NgModule, 
			Injectable, 
			Component,
			OnInit, 
			OnDestroy, 
			Output, 
			Input, 
			Pipe,
			Attribute,
			Directive,
			AfterViewInit, 
			ElementRef,
			Renderer,
			ViewChild
		} from '@angular/core';


	# HTTP:
		- import { HttpClient, HttpHeaders, HttpErrorResponse } from '@angular/common/http';

		- import { CommonModule } from '@angular/common'

	# ROUTER:
		- import { 
			Routes, 
			RouterModule, 
			Router, 
			ActivatedRoute, 
			Params, 
			CanActivate, 
			ActivatedRouteSnapshot, 
			RouterStateSnapshot, 
			CanDeactivate
		} from '@angular/router';

	# Forms:
		- import { FormBuilder, FormGroup, Validators } from '@angular/forms';

	# Rxjs:
		- import { Observable, Subscription, Subject, ErrorObserver, asapScheduler, pipe, of, from, interval, merge, fromEvent } from 'rxjs';
		
		- import { map, first, filter, scan } from 'rxjs/operators';


	# Animations:
		- import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
		- import { trigger, CSSstyle, state, style, transition Timetransition, animate, somekeyframes, generatedquery, stagger } from '@angular/animations';

